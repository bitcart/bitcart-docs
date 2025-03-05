# Bitcart Plugins Development

### Creating a New Plugin

Bitcart plugins are modular components that can extend the functionality of your Bitcart instance. A plugin can consist of one or more of the following components:

* **Backend**: Server-side logic, database models, and API endpoints
* **Admin**: Extensions to the admin panel UI
* **Store**: Customizations for the store frontend
* **Docker**: Docker compose deploy customizations

To create a new plugin, use the [Bitcart CLI](https://github.com/bitcart/bitcart-cli):

```bash
bitcart-cli plugin init .
```

This will guide you through creating a new plugin by asking for:

* Plugin name
* Author
* Description
* Component types to include

To install plugin to develop alongside bitcart, you can use `bitcart-cli plugin install .`

It is recommended to pass `--dev` flag to install plugin in development mode. It uses symlinks to link the plugin to the bitcart source code, so when you make changes to the plugin, they are reflected immediately.

It asks for paths where you cloned bitcart, bitcart-admin and other repositories. You can pass `--save` flag to save paths to config, so that it is never asked again.

After you are done developing and want to remove it from codebase:

```bash
bitcart-cli plugin uninstall your_plugin_name
```

### Backend Plugin Development

#### Plugin Structure

A backend plugin must have a `plugin.py` file which as a `Plugin` class that implements the `BasePlugin` class from `api.plugins`. The basic structure is:

```python
from api.plugins import BasePlugin

class Plugin(BasePlugin):
    name = "your_plugin_name"
    
    def setup_app(self, app):
        # Configure FastAPI app, add routes
        pass
        
    async def startup(self):
        # Initialize plugin, register hooks
        pass
        
    async def shutdown(self):
        # Cleanup resources
        pass
        
    async def worker_setup(self):
        # Setup background tasks
        pass
```

#### Database Models

To add custom database tables:

1. Create a `models.py` file in your plugin directory
2. Define SQLAlchemy models
3. Manage migrations using alembic:

```bash
python scripts/pluginmigrate.py your_plugin_name revision --autogenerate -m "Migration message"
```

```bash
python scripts/pluginmigrate.py your_plugin_name upgrade head
```

Basically same commands as alembic, but you provide your plugin name.

This will create a new migration in the `versions/` directory. Migrations are automatically applied when the plugin is loaded.

#### Hooks and Filters

Bitcart provides two extension mechanisms:

1. **Hooks**: Execute actions at specific points
2. **Filters**: Modify data as it flows through the system

```python
from api.plugins import register_hook, register_filter

# Register a hook
register_hook("event_name", async_handler_function)

# Register a filter
register_filter("filter_name", async_filter_function)
```

To find hooks and filters, you can search the codebase for `apply_filters` and `run_hook`.

For example, `db_modify_wallet` would be called right after wallet is saved in db.

So you can use it like this:

```python
from api.plugins import register_hook

class Plugin(BasePlugin):
    ...

    async def startup(self):
        register_hook("db_modify_wallet", self.my_hander)

    async def my_handler(self, wallet):
        print(wallet)
```

And in filters first argument you receive is the value which you can return unmodified or modify. Other arguments are optional data

Example:

```python
from api.plugins import register_filter, SKIP_PAYMENT_METHOD

class Plugin(BasePlugin):
    ...

    async def startup(self):
        register_filter("create_payment_method", self.create_payment_method)

    async def create_payment_method(self, method, wallet, coin, amount, invoice, product, store, lightning):
        # multiple options:
        # return SKIP_PAYMENT_METHOD # skip this method, e.g. if error occured
        # return method # fallback to bitcart default code, e.g. call add_request on the daemon
        return {
          "payment_address": "youraddress",
          "payment_url": "willbeinsideqrcode",
          "lookup_field": "for your reference",
          "metadata": {"somekey": "somevalue"},
        }
```

#### Plugin Settings

To add custom settings for your plugin:

```python
from pydantic import BaseModel
from api.plugins import register_settings

class PluginSettings(BaseModel):
    setting1: str = ""
    setting2: bool = False

register_settings("your_plugin_name", PluginSettings)
```

Settings are automatically available in the admin panel and can be accessed via:

```python
from api.plugins import get_plugin_settings

settings = await get_plugin_settings("your_plugin_name")
```

#### Dependencies

If you need to add backend dependencies, create a requirements.txt file in your plugin directory, it will be installed automatically.

```
your_plugin_name/
├── requirements.txt
```

### Frontend Plugin Development

#### Admin Panel Extensions

Admin plugins can extend the UI using the our plugin system based on [vuems](https://github.com/ergonode/vuems). The basic structure is in e.g. `admin/your_plugin_name`:

```
your_plugin_name/
├── config/
│ ├── extends.js # Component and dictionary registration
│ ├── index.js # Plugin configuration
│ └── routes.js # Custom routes
├── pages/ # Custom pages
└── components/ # Custom components
└── package.json # dependencies
```

#### Extending the UI

1. Register custom components in `extends.js`:

```javascript
export default {
  extendComponents: {
    // Add components to specific UI slots
    'dashboard-widgets': [MyWidget],
    'invoice-details': [CustomInvoiceInfo]
  },
  dictionaries: {
    // Add custom data dictionaries
    'my-data': { /* ... */ }
  }
}
```

2. Add custom routes in `routes.js`:

```javascript
import CustomPage from '../pages/CustomPage'

export default [
  {
    name: 'custom-page',
    path: '/custom',
    component: CustomPage
  }
]
```

3. Add dependencies to `package.json`

#### Store Extensions

Store plugins follow a similar structure to admin plugins but are specifically for the store frontend. They can:

* Add custom pages
* Extend existing components
* Add new payment methods
* Customize the checkout process

### Plugin Packaging

To package your plugin:

```bash
bitcart-cli plugin package .
```

This creates a `.bitcart` file that can be installed on any Bitcart instance.

### Best Practices

1. Use descriptive names for hooks and filters
2. Document your plugin's requirements and dependencies
3. Handle errors gracefully
4. Clean up resources in the shutdown method
5. Follow the existing code style
6. Test your plugin thoroughly before distribution

### Plugin Lifecycle

1. Plugin discovery and loading
2. Database migrations
3. Plugin initialization (startup)
4. Hook/filter registration
5. Settings registration
6. UI component registration (if applicable)
7. Background worker setup (if needed)
8. Shutdown cleanup on server stop
