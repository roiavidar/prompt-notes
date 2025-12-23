# Error Handling Guidelines

> **Stack Context:** These guidelines focus on GraphQL/Apollo error handling patterns. Adapt to your data layer (REST, tRPC, etc.) as needed.

**Philosophy**: Graceful degradation, clear user feedback, systematic retry logic

**Stack**: Apollo GraphQL • REST (useLazyFetch) • RetryLink • Toast • apiCodeVar

---

## Core Principles

1. **User-Centric Feedback** - Clear, actionable messages (not technical jargon)
2. **Centralized System** - `apiCodeVar` + Toast for consistency
3. **Graceful Degradation** - Partial failures don't crash workflows
4. **Audit Trail** - `auditId` for support escalation
5. **Validate Early** - Client-side before server-side

---

## GraphQL Errors

### Global Error Link

```typescript
const errorLink: ApolloLink = onError(({ graphQLErrors, networkError, response }) => {
  if (networkError) {
    apiCodeVar({ code: shouldSkipRetry(errorCode) ? 500 : 600 })
  } else if (graphQLErrors?.length > 0) {
    const error: any = graphQLErrors[0]
    apiCodeVar({ 
      code: error.code || ERROR_CODE.GRAPHQL_INPUT_ERROR, 
      auditId: response?.extensions?.auditId 
    })
  }
})
```

**When**: All GraphQL operations  
**Why**: Centralized handling, prevents duplication  
**Audit ID**: Included for 5xx errors (user can copy from Toast)

### Mutations

```typescript
const [updateEntity] = useUpdateEntityMutation({
  refetchQueries: () => [{ query: AllEntitiesDocument }],
  onCompleted: (data) => {
    if (data) {
      apiCodeVar({ code: API_CODE_SUCCESS.ITEM_UPDATED })
      closeDialog()
    }
  },
})
```

**Pattern**: `onCompleted` for success + cleanup  
**Error**: Global errorLink handles Toast (use `onError` only for specific logic)

### Queries with Navigation

```typescript
const { data } = useItemDetailsQuery({
  onError: (e: ApolloError) => {
    const code = e.graphQLErrors[0]?.code
    if (code === ERROR_CODE.ITEM_NOT_FOUND) navigateTo("/not-found")
  },
})
```

**When**: Business logic errors (404, permission, wrong service)  
**Why**: Redirect instead of generic error toast

### Polling

```typescript
const { stopPolling } = useTableSchemaDiffQuery({
  pollInterval: TIME.THIRTY_SECONDS,
  onCompleted: (data) => {
    if (data.status === FAILED || attemptCounter++ >= MAX_ATTEMPTS) {
      apiCodeVar({ code: ERROR_CODE.MAX_ATTEMPTS_REACHED })
      stopPolling()
    }
  },
})

useEffect(() => () => stopPolling(), [stopPolling])
```

**When**: Long-running operations  
**Max Attempts**: 10 (prevent infinite polling)  
**Cleanup**: Stop polling on unmount

---

## REST Errors

### useLazyFetch Hook

```typescript
const [uploadFetch] = useLazyFetch<IUploadRes>()

const onError = () => apiCodeVar({ code: API_CODE_NETWORK_ERROR.SERVER_ERROR })

const uploadFiles = async (files: File[]) => {
  const formData = new FormData()
  files.forEach((f) => formData.append("file", f))
  const { accessToken } = await getToken()

  const uploadRes = await uploadFetch(url, {
    method: "POST",
    body: formData,
    headers: { Authorization: `Bearer ${accessToken}` },
  }, SerializeTo.JSON, onError)
  
  if (!uploadRes.data) return
  
  if (uploadRes.data.failures.length) {
    apiCodeVar({ code: RestResCodes.badRequest })
  } else {
    apiCodeVar({ code: API_CODE_SUCCESS.UPLOAD_SUCCESS })
  }
}
```

**Pattern**: Pass error handler as callback  
**When**: File uploads, external APIs, non-GraphQL endpoints

### Batch Uploads

```typescript
const handleBatches = async (filesBs: File[][]) => {
  let uploadErrors: IUploadError[] = []
  
  for (const filesBatch of filesBs) {
    const uploadRes = await handleUploadBatch(filesBatch, token)
    
    if (!uploadRes.isSuccess) {
      uploadErrors = [...uploadErrors, ...uploadRes.result.errors]
    }
  }
  
  const uploaded = totalToUpload - uploadErrors.length
  
  if (uploaded === totalToUpload) {
    apiCodeVar({ code: API_CODE_SUCCESS.UPLOAD_SUCCESS })
  } else if (uploaded > 0) {
    // Warning toast: partial success
  } else {
    // Error toast: all failed
  }
}
```

**Pattern**: Collect partial failures, show aggregate feedback  
**User Feedback**: Success (10/10) • Warning (5/10) • Error (0/10)  
**Details**: Provide error dialog with table + copy-to-clipboard

---

## Retry Logic

### Apollo RetryLink

```typescript
const retryLink = new RetryLink({
  attempts: {
    max: 9,
    retryIf: (error, operation) => {
      if (error?.response?.status === 401) logout()
      return !shouldSkipRetry(errorCode, operation) && !!error
    },
  },
  delay: (count) => count * 1000, // Linear: 1s, 2s, 3s...
})

const shouldSkipRetry = (errorCode, operation) =>
  (!errorCode && operation.operationName === "ChatQuery") ||
  [504].includes(errorCode) // Skip timeouts
```

**Max Attempts**: 9  
**Delay**: Linear backoff (1s intervals)  
**Skip**: Timeouts (504), specific operations  
**Logout**: Trigger on 401

### Manual Polling

```typescript
let attemptCounter = 0
const MAX_ATTEMPTS = 10

useQuery({
  pollInterval: TIME.THIRTY_SECONDS,
  onCompleted: (data) => {
    if (data.status === FAILED || attemptCounter++ >= MAX_ATTEMPTS) {
      apiCodeVar({ code: ERROR_CODE.MAX_ATTEMPTS_REACHED })
      stopPolling()
    }
  },
})
```

**Pattern**: Counter + interval + max attempts  
**Cleanup**: Stop polling on unmount

---

## User Feedback

### Toast System

```typescript
// Trigger
apiCodeVar({ code: API_CODE_SUCCESS.UPLOAD_SUCCESS })
apiCodeVar({ code: ERROR_CODE.SERVER_ERROR, auditId: 12345 })

// Toast component
const Toast = ({ msgCode, auditId }) => {
  const message = msgCode >= 400 ? errorMapper[msgCode] : successMapper[msgCode]
  
  return (
    <Snackbar autoHideDuration={10000}>
      <Alert severity={msgCode >= 400 ? "error" : "success"}>
        {message}
        {auditId && <Button>Copy Error ID: {auditId}</Button>}
      </Alert>
    </Snackbar>
  )
}
```

**Auto-dismiss**: 10s  
**Audit ID**: For 5xx errors (user can copy for support)  
**Centralized**: Single source of truth via `apiCodeVar`

### Error Codes

```typescript
// < 400: Success
API_CODE_SUCCESS = {
  ITEM_CREATED: 97,
  ITEM_UPDATED: 98,
  UPLOAD_SUCCESS: 137,
}

// >= 400: Errors
API_CODE_NETWORK_ERROR = {
  UNAUTHORIZED: 401,
  SERVER_ERROR: 500,
  TIMEOUT: 504,
  NETWORK_ERROR: 600,
}

ERROR_CODE = {
  GRAPHQL_INPUT_ERROR: 418,
  ITEM_NOT_FOUND: 442,
  MAX_ATTEMPTS_REACHED: 703,
}
```

**Convention**: < 400 success • 500-600 HTTP-like • 600+ custom • 700+ business logic

### Error Dialogs

```typescript
<APIUploadErrorsDialog 
  errors={[
    { filename: "data.json", type: "PARSE", messages: ["Invalid JSON"] }
  ]} 
/>
```

**When**: Batch operations with partial failures  
**Features**: Table view, copy-to-clipboard, hide/keep actions  
**Why**: Granular details (which file, what error)

### Progress Indicators

```typescript
// Simple
<LoadingButton loading={isLoading}>Upload</LoadingButton>

// Batch
<APIUploadProgressDialog totalLength={10} progressIndex={5} />
```

**When**: Operations > 2s  
**Types**: Button spinner (simple) • Dialog (batch with progress)

---

## Error Recovery

### 1. Optimistic Updates

```typescript
const [deleteMutation] = useDeleteMutation({
  optimisticResponse: { deleteItem: { id, deleted: true } },
  update: (cache, { data }) => {
    cache.evict({ id: cache.identify(data.deleteItem) })
  },
  onError: () => {
    // Apollo auto-rolls back optimistic update
    apiCodeVar({ code: ERROR_CODE.DELETION_FAILED })
  },
})
```

**When**: Deletions, status updates  
**Why**: Instant feedback, auto-rollback

### 2. Cache Eviction

```typescript
const [mutation] = useMutation({
  update(cache) {
    cache.evict({ id: "ROOT_QUERY", fieldName: "catalog" })
    cache.gc()
  },
})
```

**When**: Mutations affect multiple queries  
**Why**: Prevent stale data

### 3. Refetch Queries

```typescript
const [mutation] = useMutation({
  awaitRefetchQueries: true,
  refetchQueries: [{ query: AllEntitiesDocument }],
})
```

**When**: Mutations affect list/aggregate queries  
**Note**: Use `awaitRefetchQueries` for loading states

### 4. Client Validation

```typescript
const handleSave = () => {
  const nameError = getSelectNameError()
  const domainsError = getSelectAllowDomainsError()

  if (nameError || domainsError) {
    setErrors({ name: nameError, domains: domainsError })
    return // Don't submit
  }

  updateEntity({ variables: { input: entity } })
}
```

**Pattern**: Client validation → server validation → mutation  
**Why**: Prevent server errors, immediate feedback

---

## Common Scenarios

### File Upload Failures

```typescript
const handleInvalidFileType = (validTypes, file) => {
  setUploadErrors([{
    filename: file?.name || "",
    type: "PARSE",
    messages: [`Valid types: ${validTypes.join(", ")}`],
  }])
  apiCodeVar({ code: ERROR_CODE.INVALID_FILE_TYPE })
}
```

**Causes**: Size limits, invalid format, parsing errors  
**Feedback**: Specific message, list valid formats, allow retry

### Permission Errors

```typescript
const isPermissionError = [401, 416, 600].includes(code)

{isPermissionError ? <ErrorBoundary /> : <Toast />}
```

**Handling**: Full-page error boundary (not toast)  
**Actions**: Logout button, contact admin message

### Network Timeouts

**Handling**: Retry with backoff, skip retry on 504  
**Feedback**: Toast shows "request timed out, please try again"

### Concurrent Mutations

```typescript
const [mutation, { loading }] = useMutation()

<Button disabled={loading} onClick={handleUpdate}>Update</Button>
```

**Prevention**: Disable button while loading, use optimistic locking

---

## Testing

### Mock GraphQL Errors

```typescript
const errorMock: UpdateEntityMutationMock = {
  request: { query: UpdateEntityDocument, variables: { input } },
  error: new Error("Network error"),
}

test("handles error", async () => {
  const { user } = customRender(<Form />, { mocks: [errorMock] })
  await user.click(screen.getByRole("button"))
  expect(await screen.findByText(/error/i)).toBeInTheDocument()
})
```

### Mock REST Errors

```typescript
jest.spyOn(global, "fetch").mockRejectedValue(new Error("Upload failed"))
```

### Test Retry Logic

```typescript
let attemptCount = 0
jest.spyOn(global, "fetch").mockImplementation(() => {
  if (++attemptCount < 3) return Promise.reject(new Error("Fail"))
  return Promise.resolve(new Response(JSON.stringify({ success: true })))
})
```

---

## Best Practices

### ✅ DO

- **Centralize** - `apiCodeVar` + Toast for consistency
- **Audit IDs** - Include for 5xx errors
- **Validate early** - Client-side before server-side
- **Partial success** - "5/10 uploaded" vs "failed"
- **Context** - Which file, what field, why
- **Enable recovery** - Retry button, clear error, help link
- **Test errors** - Mock in tests
- **Log** - `console.error` dev, audit logs prod

### ❌ DON'T

- **Technical errors** - "ECONNREFUSED" → "Network error"
- **Silent failures** - Always notify user
- **Duplicate handling** - Global errorLink handles GraphQL
- **Generic messages** - "Error" → "File upload failed: invalid JSON"
- **Block UI** - Set max retries, timeouts
- **Retry forever** - Max 9 (Apollo), 10 (polling)
- **Crash** - Use Error Boundaries, graceful degradation

---

## Checklists

### Mutation
- [ ] `onCompleted` with success Toast
- [ ] `refetchQueries` or cache eviction
- [ ] Client validation before submit
- [ ] Loading state (disable button)
- [ ] Test error scenario

### File Upload
- [ ] File type validation
- [ ] Size limit check
- [ ] Progress indicator (multi-file)
- [ ] Error dialog for failures
- [ ] Partial success handling
- [ ] Copy-to-clipboard for errors
- [ ] Test with invalid file

### Polling
- [ ] `pollInterval` set
- [ ] Max attempts limit
- [ ] Stop on unmount (`useEffect` cleanup)
- [ ] Status check in `onCompleted`
- [ ] Timeout feedback
- [ ] Test max attempts scenario

