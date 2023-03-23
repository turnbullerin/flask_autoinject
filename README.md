# Flask AutoInject

Flask integration to provide separate contexts 
for each request regardless of the underlying 
WSGI handler.

As of version 1.1.0, you MUST use the `init_app()` function
to properly use `autoinject` with Flask contexts. Previously,
the informant was registered automatically. Calling `init_app()` 
also registers a `teardown_appcontext()` callback which removes the 
application context.

Note that Flask calls functions registered via `teardown_appcontext()` 
from last to first. The call to `flask_autoinject.init_app()` should 
therefore go BEFORE any function which might also register an
appcontext teardown that relies on an injected variable. Since
request teardowns happen before appcontext teardowns, it is also safe
to use injected objects in request teardown functions.

```python
import flask
import flask_autoinject

app = flask.Flask(__name__)
flask_autoinject.init_app(app)
```
