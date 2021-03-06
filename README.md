# Brunt
Unofficial python SDK for Brunt, based on the npm version here https://github.com/MattJeanes/brunt-api

This package allows you to control your Brunt devices from code.

## Sample script

This script shows the usage and how to use the output of the calls, off course if you already know the name of your device you do not need to call getThings.

This script checks the current position of a blind called 'Blind' and if that is 100 (fully open), sets it to 90 and vice versa.

```python
from brunt.brunt import BruntAPI

bapi = BruntAPI()
print("Calling Brunt")

bapi.login('username', 'password')
print("    Logged in, gettings things.")

things = bapi.getThings()['things']
print(f"    { len(things) } thing(s) found.")

state = bapi.getState(thing='Blind')['thing']
print(f"    Current status of { state['NAME'] } is position { state['currentPosition'] }")
newPos = 100
if int(state['currentPosition']) == 100:
    newPos = 90
print(f"    Setting { state['NAME'] } to position { newPos }")

res = bapi.changeRequestPosition(newPos, thing='Blind')
print('    Success!' if res else '    Fail!')
    
```
<h1 id="brunt.brunt.BruntAPI">BruntAPI</h1>

```python
BruntAPI()
```
Constructor for the API wrapper.

If you supply username and password here, they are stored, but not used.
Auto logging in then does work when calling another method, no explicit login needed.

:param username: the username of your Brunt account
:param password: the password of your Brunt account


<h2 id="brunt.brunt.BruntAPI.login">login</h2>

```python
BruntAPI.login(self, username, password)
```
Login method using username and password

:param username: the username of your Brunt account
:param password: the password of your Brunt account

:return: True if successfull
:raises: errors from Requests call

<h2 id="brunt.brunt.BruntAPI.getThings">getThings</h2>

```python
BruntAPI.getThings(self)
```
Get the things registered in your account

:return: dict with things registered in the logged in account and API call status
:raises: errors from Requests call

<h2 id="brunt.brunt.BruntAPI.getState">getState</h2>

```python
BruntAPI.getState(self, **kwargs)
```
Get the state of a thing

:param thing: a string with the name of the thing, which is then checked using getThings.
:param thingUri: Uri (string) of the thing you are getting the state from, not checked against getThings.

:return: a dict with the state of the Thing.
:raises: ValueError if the requested thing does not exists. NameError if not logged in. SyntaxError when
    not exactly one of the params is given.

<h2 id="brunt.brunt.BruntAPI.changeRequestPosition">changeRequestPosition</h2>

```python
BruntAPI.changeRequestPosition(self, request_position, **kwargs)
```
Changes the position of the thing. Internally calls the changeKey method with key set to
requestPosition and value set to request_position

:param request_position: The new position for the slide (0-100)
:param thing: a string with the name of the thing, which is then checked using getThings.
:param thingUri: Uri (string) of the thing you are getting the state from, not checked against getThings.

:return: a dict with the state of the Thing.
:raises: ValueError if the requested thing does not exists or the position is not between 0 and 100.
    NameError if not logged in. SyntaxError when not exactly one of the params is given.

<h2 id="brunt.brunt.BruntAPI.changeKey">changeKey</h2>

```python
BruntAPI.changeKey(self, **kwargs)
```
Change a variable of the thing by supplying the key and value. Mostly included for future additions.

:param key: The key of the value you want to change
:param value: The new value
:param thing: a string with the name of the thing, which is then checked using getThings.
:param thingUri: Uri (string) of the thing you are getting the state from, not checked against getThings.
:return: a dict with the state of the Thing.
:raises: ValueError if the requested thing does not exists or the position is not between 0 and 100. 
    NameError if not logged in. SyntaxError when not exactly one of the params is given. 
