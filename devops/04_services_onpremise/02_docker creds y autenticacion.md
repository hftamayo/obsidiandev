To determine the authentication methods and credentials that Docker is using, you can inspect the Docker configuration files and the Docker credential store. Here are some steps to help you identify the authentication methods Docker is using:

### 1. Inspect Docker Configuration

Docker stores its configuration in the `~/.docker/config.json` file. This file contains information about the Docker credentials and the credential store being used.

#### View Docker Configuration

```sh
cat ~/.docker/config.json
```

### Example `config.json`

```json
{
  "auths": {
    "https://index.docker.io/v1/": {
      "auth": "base64-encoded-auth-string"
    }
  },
  "credsStore": "pass",
  "HttpHeaders": {
    "User-Agent": "Docker-Client/19.03.12 (linux)"
  }
}
```

### 2. Check Credential Store

If the `config.json` file specifies a `credsStore`, Docker is using a credential helper to manage credentials. The `credsStore` field indicates which credential helper is being used (e.g., `pass`, `osxkeychain`, `wincred`).

### 3. Decode Base64-Encoded Credentials

If the `auths` section contains a base64-encoded `auth` string, you can decode it to see the username and password.

#### Decode Base64-Encoded Auth String

```sh
echo "base64-encoded-auth-string" | base64 --decode
```

### 4. List Docker Credentials

If Docker is using a credential helper, you can list the stored credentials using the appropriate command for the credential helper.

#### Example for `pass` (Linux)

```sh
pass ls
```

#### Example for `osxkeychain` (macOS)

```sh
security find-internet-password -s "index.docker.io" -w
```

#### Example for `wincred` (Windows)

```sh
cmdkey /list
```

### Summary

1. **Inspect Docker Configuration**:
    ```sh
    cat ~/.docker/config.json
    ```

2. **Check Credential Store**: Look for the `credsStore` field in the `config.json` file to determine which credential helper is being used.

3. **Decode Base64-Encoded Credentials**:
    ```sh
    echo "base64-encoded-auth-string" | base64 --decode
    ```

4. **List Docker Credentials**: Use the appropriate command for the credential helper being used (e.g., `pass`, `osxkeychain`, `wincred`).

By following these steps, you can determine the authentication methods and credentials that Docker is using for the `docker login` command. This will help you understand how Docker is managing your credentials and where they are stored.

### ==Verificar como estoy autenticandome cada vez que ejecuto un docker login

The `auth` field in your `config.json` file contains a base64-encoded string that represents your Docker credentials. You can decode this string to see the username and password.

### Steps to Decode the Base64-Encoded Auth String

1. **Copy the Base64-Encoded Auth String**:
    ```plaintext
    aGZ0YW1heW9AZXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    ```

2. **Decode the String**:
    Use the `echo` command with `base64 --decode` to decode the string.

    ```sh
    echo "aGZ0YW1heW9AZ2XXXXXXXXXXXXXXXXXXXXX==" | base64 --decode
    ```

3. **View the Decoded Credentials**:
    The decoded string will be in the format `username:password`.

### Example

```sh
echo "aGZ0YW1heW9AXXXXXXXXXXXXXXXX" | base64 --decode
```

Output:
```plaintext
hftamayo@gmail.com:dckr_pat_iTyXXXXX
```

In this example, the username is `hftamayo@gmail.com` and the password is `dckr_pat_iXXXXX`.

### Summary

1. **Copy the Base64-Encoded Auth String** from your `config.json` file.
2. **Decode the String** using the `echo` command with `base64 --decode`.
3. **View the Decoded Credentials** to see the username and password.

By following these steps, you can decode the base64-encoded Docker credentials stored in your `config.json` file and understand what credentials Docker is using for authentication.