#API functions

The API provides four functions to enable the use of SSO:

* `LogonSSOUser`
* `LogonSSOUserWithPin`
* `LogoffSSOUser`
* `IsReconnectioninProgress`

##LogonSsoUser
```c
LOGONSSOUSER_ERROR_CODE APIENTRY
LogonSsoUser (
			  MSV1_0_INERACTIVE_LOGON 			pNewCredentials,
			  BOOL 							    notUsed,
			  BOOL 								notUsed2,
			  DWORD 							*pDwResult
			  )
```

This function is used to provide user credentials to SSO. If this function is invoked from a service, the service should be able to call these APIs in the locally logged-on user’s session.

| Parameter         | Description                                                                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `pNewCredentials`   | This parameter is a pointer to the `MSV1_0_INTERACTIVE_LOGON` structure, which provides the users credentials to SSO.  See [http://msdn.microsoft.com/en- us/library/aa378760(v=VS.85).aspx]() for further details on this structure. |
| `notUsed`, `notUsed2` | These parameters are deprecated.                                                                                                                                                                                                |
| `pDwResult`         | Pointer to a `DWORD` to contain the result code of this operation.                                                                                                                                                                |

| Return value                          | Description                      |
|---------------------------------------|----------------------------------|
| LOGONSSOUSER_OK                       | Operation completed succesfully. |
| LOGONSSOUSER_UNABLE_TO_GET_PIPE_NAME  | Error communicating with SSO.    |
| LOGONSSOUSER_UNABLE_TO_CONNECT_TO_SSO | Error communicating with SSO.    |
| LOGONSSOUSER_UNABLE_TO_SEND_REQUEST   | Error communicating with SSO.    |
| LOGONSSOUSER_INVALID_RESPONSE         | Error in SSO response.           |                                                 

##LogonSsoUserWithPin
```c
BOOL APIENTRY DoLogonSsoUserWithPin
```

This function is used to provide smart card user credentials to SSO.

| Parameter | Description |
|-----------|-------------|
| `pin`		| The smart card PIN |

| Return value | Description |
|-----------|-------------|
| BOOLEAN		| `TRUE` if operation completed successfully, `FALSE` otherwise. |

##LogoffSSOUser                                                                      
```
DWORD APIENTRY
				DWORD 			notUsed
```
This function removes the credentials of the current SSO user and restores the previous user’s credentials if available.

| Parameter | Description |
|-----------|-------------|
| `notUsed`		| This parameter is deprecated. |

| Return value | Description |
|-----------|-------------|
| LOGONSSOUSER_OK		| Operation completed succesfully. | 
| LOGONSSOUSER_UNABLE_TO_GET_PIPE_NAME | Error communicating with SSO. |
| LOGONSSOUSER_UNABLE_TO_CONNECT_TO_SSO | Error communicating with SSO. |
| LOGONSSOUSER_UNABLE_TO_SEND_REQUEST | Error communicating with SSO. |
| LOGONSSOUSER_INVALID_RESPONSE | Error in SSO response. |                                                                              

##IsReconnectInProgress
```
DWORD APIENTRY
This function works with auto client reconnect to determine whether a reconnect is being attempted after the last logon using `LogonSsoUser()`. You must call it immediately after `LogonSsoUser`. If there is a session to reconnect, the auto client reconnect feature issues the reconnect request. The API communicates that a new reconnect request has been issued; therefore no new session launch request is needed.



| Parameter | Description |
|-----------|-------------|
| `dwTimeout` | The maximum number of millisconds to wait to determine whether a reconnect is in progress. |
| `lpReconnectInProgress` | Points to a flag that is set to TRUE if the reconnect request was launched after the last logon or to FALSE if there was no session to reconnect. |

| Return value | Description |
|--------------|-------------|
| ERROR_SUCCESS | Operation completed successfully |
| ERROR_TIMEOUT | The timeout period expired or another relevant error code was issued. |
