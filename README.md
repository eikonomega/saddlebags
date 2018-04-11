# Saddlebags

Taking the pain out of credential and program configuration management for
your Python apps.

Saddlebags allows you to easily load JSON and YAML for use in your programs.
Never be tempted to hard-code your credentials or service configurations again!

## Usage

1.  Assume that you have the following configuration files in `/home/user`:

    ```json
    # ldap.json
    {
        "server": "mycoolldap.superradcompany.com",
        "username": "forestGump",
        "password": "ilikechocolate"
    }
    ```

    ```yaml
    # oracle.yaml
    ---
    mycooldatabase:
        username: ironman
        password: jarvis
    ```

1.  And a environment variable that points to `home/user`. For example:
    `export MY_COOL_ENVVAR=/home/user`

1.  Then you can do this in your program:

    ```python
    import saddlebags
    saddlebag = saddlebags.Saddlebag(['MY_COOL_ENVVAR'])
    ```

1.  The program will go out, find and parse your config files, and make their
    contents available like so:

        ```python
        >>> saddlebag['ldap']['server']
        "mycoolldap.superradcompany.com"

        >>> saddlebag['oracle']['mycooldatabase']['password']
        "jarvis"
        ```

1.  The default behavior of Saddlebags is to raise a KeyError if you
    ask for data that hasn't been loaded:

    ```python
    >>> saddlebag['does-not-exist']['server']
    KeyError: "The requested key 'does-not-exist' does not exist. This most likely indicates that you anticipated a configuration file being loaded that actually hasn't been."
    ```

1.  If you prefer, you can have a Saddlebag return None by setting the
    `strict` parameter to `False`:

    ```python
    import saddlebags
    saddlebag = saddlebags.Saddlebag(['MY_COOL_ENVVAR'], strict=False)

    value = saddlebag['does-not-exist']['server']

    value is None
    True
    ```

### Bonus: Environment Variables Access!

You can access environment variables via the 'env' attribute of any
Saddlebag object:

```python
# Assuming `MY_COOL_ENVVAR` exists and you've create a Saddlebag object.
saddlebag.env['MY_COOL_ENVVAR']
```
