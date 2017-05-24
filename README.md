# Saddlebags
Taking the pain out of credential and program configuration management for
your Python apps.
  
Saddlebags allows you to easily load JSON and YAML for use in your programs.
Never be tempted to hard-code your credentials or service configurations again!

## Usage
1. Assume that you have the following configuration files in `/home/user`:

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
    
1. And a environment variable that points to `home/user`. For example: 
`export MY_COOL_ENVVAR=/home/user`

1. Then you can do this in your program:
    
    ```python
    import saddlebags
    saddlebag = saddlebags.Saddlebag(['MY_COOL_ENVVAR'])
    ```
    
1. The program will go out, find and parse your config files, and make their
contents available like so:

    ```python
    >>> saddlebag['ldap']['server']
    "mycoolldap.superradcompany.com"
    
    >>> saddlebag['oracle']['mycooldatabase']['password']
    "jarvis"
    ```

### Bonus!: Environment Variables Access
You can access environment variables via the 'env' attribute of any
Saddlebag object:

```python
# Assuming `MY_COOL_ENVVAR` exists and you've create a Saddlebag object.
saddlebag.env['MY_COOL_ENVVAR']
```