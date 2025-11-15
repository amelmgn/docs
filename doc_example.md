# Collections

## Description

Directus collections help you manage your data. Create custom collections, define fields, configure relationships, and leverage features like content versioning and access control. Learn how to create, configure, and manage collections in Directus.

Collections are database tables with additional metadata and configuration used by Directus.

All Directus-specific settings and data is held only in system collections prefixed with `directus_`. Should you wish to remove Directus, you can remove these collections with no proprietary data left behind.

There are two types of collections: **user collections** and **system collections**.

## Creating Collections

**User collections** are created directly in your database or via Directus. They describe your specific data models and configurations and can be queried via APIs created by :product-link{product="connect"} or via :product-link{product="explore"}.

You can create collections from the Data Model settings in the Directus Data Studio, or using the [Collections API](/api/collections).

### Name

The unique name for your collection must be set on creation, is case sensitive, and will be used throughout the Data Studio and Directus APIs.

**Collection names are immutable**
The collection name cannot be modified after collection creation, which includes casing. However, you can override how it is displayed in the Data Studio with collection naming translations.

### Primary Key

The primary key field contains a unique value for each item in the collection. This unique identifier is used in a number of places:

- When creating [relationships](/guides/data-model/relationships) between items across different collections.
- When fetching or performing actions on data using the [Items API](/api/items).

The primary key type defines the strategy for creating unique primary keys. Once selected, this cannot be modified.

Additionally, Directus can create some common fields, including a status field, sort, as well as fields related to who and when items are created or edited. Directus supports the following types of IDs:

- **Auto-Incremented Integer**: IDs increment from `1` to `2^31-1` (`2,147,483,647`).
- **Auto-Incremented Big Integer**: IDs increment from `1` to `2^63-1` (`9,223,372,036,854,775,807`). This is only available if using MySQL and PostgreSQL as your database.
- **Generated UUID**: Universally Unique Identifier. Creates a completely unique ID. IDs generated with this system (not just in your database, but anywhere this system is used) are so statistically unlikely to be repeated that for all practical purposes they are unique.
- **Manually Entered String**: you manually type out a unique string as the ID for each item. Directus will ensure they are unique by forbidding new item creation with a duplicate ID.

## Configuring Collections

Once a collection is created, there are a number of configuration options available.

### Collection Setup

- **Note**: set a helpful note that explains the collection's purpose. Seen in Data Model settings page.
- **Icon**: icon used throughout the Data Studio when referencing this collection.
- **Color**: color for the icon, shown in the navigation bar and page headers.
- **Display Template**: display templates are used to represent an item in relationship fields - for example to show the value of the `Name` field when displaying a post's author.
- **Hidden**: toggle whether the collection should be globally hidden in the Data Studio.
- **Singleton**: toggle to bypass the collection page and take users to the single item details page.
- **Collection Naming Translations**: translate the collection name across multiple languages. When the default language is changed, the relevant translation will be used throughout the Data Studio.

### Content Versioning

Content versioning is used by :product-link{product="editor"} and allows teams to create and manage different versions of their content. There are several reasons to use content versioning, including drafting content without publishing it, and more ways to collaborate with others.

This feature can be enabled for specific collections, and will be available for all items. Once enabled, item versions can be created, and later have some or all fields promoted to the main version, typically used for publishing.

### Live Preview

Live preview is used by :product-link{product="editor"} and allows for your application to be shown in a pane next to your content, which can be used for previewing content before publishing.

You can use item field values to construct the URL used by the live preview, including unique identifiers and content version, allowing for previewing content versions before promoting them to the main version.

### Accountability

**Revisions** are created whenever an item is updated. This history of versions is tracked so that previous states can be recovered. Every change made to items in Directus is stored as a complete versioned snapshot and a set of specific changes made.

**Activity** is a log of all events that take place within the platform. Each activity record tracks the event type, user, timestamp, IP address, user-agent, and any associated revision data.

By default, your Directus project tracks all activity and revisions for collections. However, you can override this and choose to only track activity, or nothing.

**Accountability vs Telemetry**
Accountability is a log of who does what in your project. It is for your team's own use. This is different from telemetry, which is configured in environment variables.

### Sorting

The sort feature enables users to manually sort items in the Data Studio and via the API. If not selected as an optional field when creating a collection, you will need to create a new field with an integer type. It can then be selected as the sort field.

**Sorting Relational Fields**
To configure manual sorting within a [relational field](/guides/data-model/relationships) (e.g., M2M, O2M, or M2A), also set the sort field within the relationship section of the field's configuration drawer.

Once configured, click "Sort Button" in the configured sort column, and then drag items by their "Drag Button" handle. You can also sort by the sort field with querying data via the [Items API](/api/items).

### Duplication

The **Save as Copy** option in :product-link{product="editor"} offers a way to effectively duplicate the current item. Duplication settings define which field values will be copied.

**Duplicating Items with Relationships**<br/>
When you duplicate an item, any related items are not copied. You must create new relationships between the duplicated item and related items.

### Archive Settings

Archived items still exist in your collections, but are filtered within the Data Studio. If not selected as an optional field when creating a collection, you will need to create a new field.

To configure an archive field, set the following four input fields as desired.

- **Archive Field**: selects the archive field.
- **Archive App Filter**: can users filter for archived items.
- **Archive Value**: assigned value to archive field when an item is archived.
- **Unarchive Value**: assigned value to archive field when an item is unarchived.

**Archived Items via API**
Archived items are hidden in the app by default, but they are still returned normally via the API unless explicitly filtered out. This gives you the flexibility to manage archived items however you want when working with the API.

### Advanced Field Creation Mode

:partial{content="advanced-field-creation-mode"}

## System Collections

**System collections** are the automatically-created collections required for Directus to operate. These collections are surfaced throughout Directus to power different parts of the system - like notifications, users, roles, and even collection configuration itself.

While fields that come system collection fields cannot be altered, you can extend system collections.

| Collection               | Purpose                                           |
| ------------------------ | ------------------------------------------------- |
| `directus_activity`      | Accountability logs for all events.               |
| `directus_collections`   | Additional collection configuration and metadata. |
| `directus_dashboards`    | Dashboards within the Insights module.            |
| `directus_extensions`    | Configuration of extensions.                      |
| `directus_fields`        | Additional field configuration and metadata.      |
| `directus_files`         | Metadata for all managed file assets.             |
| `directus_flows`         | Automation flows.                                 |
| `directus_folders`       | Provides virtual directories for files.           |
| `directus_migrations`    | What version of the database you're using.        |
| `directus_notifications` | Notifications sent to users.                      |
| `directus_operations`    | Operations that run in Flows.                     |
| `directus_panels`        | Individual panels within Insights dashboards.     |
| `directus_permissions`   | Access permissions for each role.                 |
| `directus_presets`       | Presets for collection defaults and bookmarks.    |
| `directus_relations`     | Relationship configuration and metadata.          |
| `directus_revisions`     | Data snapshots for all activity.                  |
| `directus_roles`         | Permission groups for system users.               |
| `directus_sessions`      | User session information.                         |
| `directus_settings`      | Project configuration options.                    |
| `directus_shares`        | Tracks externally shared items.                   |
| `directus_translations`  | Custom translations.                              |
| `directus_users`         | System users for the platform.                    |
| `directus_versions`      | Content Versions for items.                       |

## Existing Database Tables

When Directus is connected to an existing database, it will introspect existing tables and relationships and collections will be made available to admins via the [Items API](/api/items).

To access the collection via the Data Studio, you must configure the collection with additional Directus-specific metadata, such as interfaces, displays, and icons.

To access the collection without an admin role, you must also configure [access control](/guides/auth/access-control) as with any other collection.

Finally, any relationships not configured in the database must also be set up inside of Directus.

**Composite Keys & SQL Views**
Directus does not currently support composite keys, or the creation of virtual tables via SQL views.


# Fields

# Description

Directus fields let you define how your data is stored and displayed. Learn about creating fields, data types, interfaces, validations, relationships, and more. Discover how to configure fields to perfectly suit your data modeling needs in Directus.

Fields are database columns with additional metadata and configuration used by Directus, such as how data is displayed, validations, and interfaces.

## Types

Directus supports various databases, though each vendor has their own simple data types. To standardize these differences, Directus has a single set of types that are mapped to the vendor-specific ones.

| Group         | Types                                                                             |
| ------------- | --------------------------------------------------------------------------------- |
| Text          | `String`, `Text`, `UUID`, `Hash`, `Alias`                                         |
| Numeric       | `Integer`, `Big Integer`, `Float`, `Decimal`                                      |
| Boolean       | `Boolean`                                                                         |
| Date and Time | `Timestamp`, `DateTime`, `Date`, `Time`                                           |
| Binary        | `Binary`                                                                          |
| Structured    | `JSON`, `CSV`                                                                     |
| Geospatial    | `Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon` |

Fields that do not map directly to an actual database column are called "alias" fields, and include presentational fields and certain relational fields.

Geospatial fields are used to store data in [GeoJSON](https://geojson.org/) format. These types are only supported if your database supports spatial extensions.

## Creating Fields

When creating a new field, you must first select an [interface](/guides/data-model/interfaces) and provide some basic configuration. Basic configuration will depend on the interface selected, but all fields have some common characteristics.

The **interface** describes how users will create and edit data, as well as how it is displayed in :product-link{product="editor"}. There are many kinds of built-in interface, such as a text input, date selector, map, and a set of relationship interfaces. More interfaces can be built as [extensions](/guides/extensions/overview).

The **key** is the unique name for a field within a collection. This value is used in both the Data Studio and via API. Within the Data Studio, the key is parsed through our title formatter to improve readability.

The **type** defines the underlying data type configured in the database. When creating fields, the available types will correspond to the interface selected.

**Field names and types are immutable**
The field key and type cannot be modified after collection creation, which includes casing. However, you can override how the key is displayed in the Data Studio with field name translations.

## Configuring Fields

There are a number of advanced configuration options available for fields. Some must be configured at the time of creation, while others can be edited after creation.

### Schema

This section defines the database column settings for the field. This includes the key, type, length (if applicable), default value, whether the field is nullable, whether values must be unique in the collection, whether the field is indexed, and whether or not to include the field in searches.

### Field

This section sets details for the interface, which is displayed in the editor. This includes whether the field is read-only or requires a value to be provided when an item is created.

This section also allows for a note - which is displayed in the editor and provides additional context or help to the user, and field name translations, which changes the label of this field in the editor based on the selected language in the Data Studio.

### Interface

This section describes how the user will create, edit, and view data in the editor, and are effectively different form inputs. Each interface will have its own configuration options.

See all available interfaces in Directus.

### Display

This section describes how individual field values will be displayed throughout the Data Studio. You can always choose to display the raw value, but you can display a formatted value instead. Each interface will have its own display options.

Conditional styles allow for different colors, icons, and text values to be used when the field data meets configured criteria.

### Validation

This section allows for the configuration of rules to determine valid user input. If validation fails, you can also use this configuration section to write custom messages.

These validations apply to data added via Directus and are not database-level validations.

### Conditions

This section alters the current field's configuration based on the values of other fields in the item. Conditional configuration include whether a field is read-only, hidden, or required, along with any editable interface-specific configuration.

**Conditionally Show Fields**
To show a field conditionally, set hidden to `true` in the field settings. Then, create a condition with a rule checking the value of another field. The condition should set hidden to `false`.

### Relationships & Translations

This section will only appear when the field is relational, and describes the relationship between this field and other collections. Each relationship type will be shown differently, and where there is a junction collection you will be given the option to add a sort field on the collection.

Relational triggers configure what will happen to the relational field data when related values are deselected or deleted. This includes setting values to `null`, their default, or cascading/preventing the deletion.

See all available relationship types in Directus.

### Field Width

Each field can be configured to be half or full width in the editor. Two half-width fields can be placed next to each other. Additionally, a field can be configured to use up the full-width of the editor, expanding beyond the usual full-width container.

Field width is not in the field configuration, but can be set by clicking on :icon{name="material-symbols:more-vert" title="Field Width Button"} in the collection data model page.

**Field Width Configurations**
- `half` fields will go from `[start]` until `[half]` in the grid system, but have a maximum width of `380px` defined by the `--form-column-max-width` css variable.
- `full` fields will be the sum of 2 `half` fields, with a maximum width of `760px`.
- `fill` fields will fill the available space, without any maximum.

### Field Data Attributes

Each field in the Data Studio includes data attributes that expose the underlying data model information:
 - `data-collection` - The collection name
 - `data-field` - The field name
 - `data-primary-key` - The item ID

These attributes enable programmatic field identification for custom styling, extensions, automated testing, and custom tooling.

```html
<div data-primary-key="1"
     data-collection="books" 
     data-field="name">
  <!-- Field UI -->
</div>
```

## System Collection Fields

System collections have a number of default fields that cannot be configured as they are required for Directus to operate. However, additional fields can be added to any system collection.

## Existing Database Fields

When Directus is connected to an existing database, fields of supported types will be automatically made available in Directus. You can then fully configure it as desired within the Data Studio.


# Interfaces

## Description

Manage your data effectively with Directus fields. Discover various field types, interfaces, validations, and relationships to perfectly suit your data modeling needs.

Interfaces are how users interact with fields in :product-link{product="editor"}. Each interface supports specific data types and configurations.

## Text & Numbers

### Input

A standard form input.

| Configuration | Options                                                                |
| ------------- | ---------------------------------------------------------------------- |
| Types         | `String`, `Text`, `UUID`, `Integer`, `Big Integer`, `Float`, `Decimal` |
| Length        | Used to limit number of characters in the database.                    |
| Soft Limit    | Used to limit the number of characters within the Data Studio.         |
| Trim          | Toggle to trim whitespace at start and end of the value.               |
| Masked        | Toggle to hide the typed value with &bull; values.                     |
| Cleared Value | Toggle to save cleared value as an empty string.                       |
| Slugify       | Toggle to make the entered value URL safe.                             |

### Autocomplete Input (API)

A search input that will populate dropdown choices by making a request to a given URL.

| Configuration | Options                                                                              |
| ------------- | ------------------------------------------------------------------------------------ |
| Types         | `String`, `Text`                                                                     |
| URL           | The request URL with dynamic `{{ value }}`.                                          |
| Results Path  | The returned object path using dot notation containing an array with search results. |
| Text Path     | The value within each object that displays for each search result.                   |
| Value Path    | The value within each object that is stored for each search result.                  |
| Trigger       | Select between `throttle` and `debounce`.                                            |
| Rate          | The delay in `milliseconds` used for the trigger.                                    |

**Throttle vs Debounce**
Throttle and debounce are very similar. Debounce will wait until a period of 'silence' has happened before making the request, while throttle will keep making requests at most 1 call every period. Period is set in the 'rate' configuration for this interface.

### Block Editor

Allows users to create and edit content using blocks. These blocks represent individual pieces of content, such as text,
images, videos, buttons, and more, that can be assembled and re-arranged within a flexible layout.

| Configuration | Options                                                                 |
| ------------- | ----------------------------------------------------------------------- |
| Types         | `JSON`                                                                  |
| Toolbar       | Allows for customization of visible formatting options.                 |
| Root Folder   | Default folder to store uploaded files. Does not affect existing files. |

### Code

Code editor for pre-formatted text.

| Configuration | Options                                                                                       |
| ------------- | --------------------------------------------------------------------------------------------- |
| Types         | `String`, `Text`, `JSON`, `Geometry (All)`                                                    |
| Language      | Language for syntax highlighting. Can be set when type is not `JSON`.                         |
| Line Number   | Toggle to show line numbers.                                                                  |
| Line Wrapping | Toggle to wrap text to prevent horizontal scrolling.                                          |
| Template      | Preset value that the user can add to the field value by clicking "Fill with Template Value". |

### Textarea

Textarea input for longer plain text.

| Configuration | Options                                                        |
| ------------- | -------------------------------------------------------------- |
| Types         | `Text`                                                         |
| Soft Limit    | Used to limit the number of characters within the Data Studio. |

### WYSIWYG

The What You See Is What You Get (WYSIWYG) editor provides a text area with rich formatting options in the toolbar.

| Configuration       | Options  |
| Types               | `Text`                                                                                                                                                                                |
| Toolbar             | Allows for customization of visible formatting options                                                                                                                                |
| Folder              | Default folder to store uploaded files. Does not affect existing files.                                                                                                               |
| Soft Limit          | Used to limit the number of characters within the Data Studio.                                                                                                                        |
| Static Access Token | Token appended to asset URLs when displaying in the editor.                                                                                                                           |
| Custom Formats      | JSON array of formatting that is passed to the `style_formats` config option of the WYSIWYG editor instance. [TinyMCE Documentation](https://www.tiny.cloud/docs/demo/format-custom/) |
| Options Override    | JSON object to override the default config option of the WYSIWYG editor instance. [TinyMCE 

Documentation](https://www.tiny.cloud/docs/configure/)                                     |

### Markdown

Markdown text editor with formatting options in the toolbar. You can switch between Edit and Preview modes.

| Configuration       | Options                                                                              |
| ------------------- | ------------------------------------------------------------------------------------ |
| Types               | `Text`                                                                               |
| Toolbar             | Allows for customization of visible formatting options.                              |
| Root Folder         | Default folder to store uploaded files. Does not affect existing files.              |
| Static Access Token | Token appended to asset URLs when displaying in the editor.                          |
| Soft Limit          | Used to limit the number of characters within the Data Studio.                       |
| Custom Blocks       | Each block adds an icon to the toolbar, and wraps the cursor in specific characters. |

### Tags

A text input that allows users to apply any number of tags. When adding new tag, press Enter to save the tag.

| Configuration  | Options                                                                 |
| -------------- | ----------------------------------------------------------------------- |
| Types          | `JSON`, `CSV`                                                           |
| Presets        | Preset tags that the user can select.                                   |
| Alphabetize    | Toggle to force tags to display in alphabetical order.                  |
| Allow Other    | Toggle to allow users to add new tags.                                  |
| Whitespace     | Force whitespace to be removed or replaced with hyphens or underscores. |
| Capitalization | Force tags to be stored as lowercase, uppercase, or title case.         |

## Selection

### Toggle

A checkbox input that allows user to toggle boolean values.

| Configuration | Options                                            |
| ------------- | -------------------------------------------------- |
| Types         | `Boolean`                                          |
| Default Value | `true`, `false`, `null`                            |
| Icon On       | Icon that shows whenever the toggle is enabled.    |
| Icon Off      | Icon that shows whenever the toggle is disabled.   |
| Color On      | Color of the icon whenever the toggle is enabled.  |
| Color Off     | Color of the icon whenever the toggle is disabled. |
| Label         | The text displayed to the user beside the toggle.  |

### Datetime

Date picker input that allows user to select a date and time.

| Configuration      | Options                                 |
| ------------------ | --------------------------------------- |
| Types              | `DateTime`, `Date`, `Time`, `Timestamp` |
| Include Seconds    | Show seconds in the interface.          |
| Use 24-Hour Format | Use 24 hour time system.                |

**Handling Timezones**
The `DateTime` type does not have timezone information, and will always store values as UTC. The `Timestamp` type contains timezone information.

### Repeater

A repeating group of fields. You can use any field in a repeater, except for relational, presentation, or group fields. The value is stored as a JSON array of objects.

| Configuration      | Options                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------- |
| Types              | `JSON`                                                                                             |
| Template           | Label shown in the repeater items using display templates. Defaults to `{{ title }}`.              |
| "Create New" Label | Label for the button to create a new item below the field in the editor. Defaults to "Create New". |
| Sort               | Method of sorting the items groups within the repeater.                                            |
| Edit Fields        | The configuration for fields within the repeater.                                                  |

Each field in a repeater has further configuration options.

| Configuration     | Options                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------- |
| Field             | Unique name for the field - used as the property key and in the repeater's `template` option. |
| Type              | Data type of property values.                                                                 |
| Interface         | The interface to use for the field.                                                           |
| Interface Options | Option configuration for the selected interface.                                              |
| Display           | The display to use for the preview on each item.                                              |

### Map

Show and set geospatial data on an interactive map. Mapping data is stored as [GeoJSON](https://geojson.org/).

**Maps Provider**
By default, Directus will use [OpenStreetMap](https://www.openstreetmap.org) to display your mapping data.
d
| Configuration | Options |
| Types         | <ul><li>**`Point`**: A single location on a map.</li> <li>**`LineString`**: A series of points on a map connected in a line.</li> <li>**`Polygon`**: An area of a map drawn by selecting vertices.</li> <li>**`Multipoint`**: A series of disconnected points on a map.</li> <li>**`MultiLineString`**: A series of `LineString` objects.</li> <li>**`MultiPolygon`**: A series of `Polygon` objects.</li> <li>**`Geometry (All)`**: An series of `Point`, `LineString` and `Polygon` objects.</li> <li>**`JSON`**: A `Geometry (All)` object stored as JSON.</li> <li>**`String`**: A `Geometry (All)` object stored a string of characters.</li> <li>**`Text`**: A `Geometry (All)` object stored as Text.</li> <li>**`CSV`**: A `Geometry (All)` object stored as comma-separated values.</li> |
| Default View  | The default location and zoom settings on the map to show by default |

**Database Support**
Your database must support geospatial data or have a geospatial plugin installed, such as PostGIS or SpatiaLite, to fully utilize mapping functionality with Directus.

### Color

A color picker interface that allows users to input color codes and convert between different color modes.

| Configuration | Options                                           |
| ------------- | ------------------------------------------------- |
| Types         | `String`                                          |
| Opacity       | Enable opacity values to be adjusted by the user. |
| Preset Colors | Preset colors that users can select.              |

### Dropdown

Input that allows the user to select a value from a dropdown list of options.

| Configuration | Options                                                                                                       |
| Types         | `String`, `Integer`, `Big Integer`, `Float`, `Decimal`                                                        |
| Choices       | Options for the dropdown. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                   |
| Allow None    | Allow user to deselect an option.                                                                             |

### Dropdown (Multiple)

Input that allows user to select multiple values from a list of options.

| Configuration | Options                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| Types         | `JSON`, `CSV`                                                                                                 |
| Choices       | Options for the dropdown. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                   |
| Allow None    | Allow user to deselect an option.                                                                             |

### Icon

Allow the user to select from a list of icons. Icons are from the [Google Material Symbols](https://fonts.google.com/icons) library and cannot be swapped for another library.

| Configuration | Options  |
| ------------- | -------- |
| Types         | `String` |

### Checkboxes

Input that allows user to select multiple checkboxes.

| Configuration | Options                                                                                                         |
| ------------- | --------------------------------------------------------------------------------------------------------------- |
| Types         | `JSON`, `CSV`                                                                                                   |
| Choices       | Options for the checkboxes. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                     |
| Color         | Color of the checkboxes.                                                                                        |
| Icon On/Off   | Icons shown whenever a checkbox is checked/unchecked.                                                           |
| Items Shown   | Number of checkboxes before a 'Show More' toggle is shown.                                                      |

### Checkboxes (Tree)

Nested tree of checkboxes that can be expanded or collapsed.

| Configuration   | Options                                                                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Types           | `JSON`, `CSV`                                                                                                                                 |
| Choices         | Options for the checkboxes. Each option contains text that is displayed to the user and a value that is stored, along with any child options. |
| Choice Children | Child checkboxes nested below the current choice.                                                                                             |
| Value Combining | Controls what value is stored when nested selections are made. `All`, `Branch`, `Leaf`, `Indeterminate`, `Exclusive`.                         |

**Understanding Value Combining Options**
In a Checkboxes (Tree) interface, checkboxes can exist within a parent checkbox. Value combining determines the final value when selecting items in a tree.
- `All` returns all checked values.
| Selection                                                                                                                | Final Value                |
| ------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`                 |
| :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2, parent]` |
- `Branch` returns the top-most values that are selected.
| Selection                                                                                                                | Final Value |
| ------------------------------------------------------------------------------------------------------------------------ | ----------- |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`  |
| :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[parent]`  |
- `Leaf` returns the deepest values that are selected
| Selection                                                                                                                | Final Value        |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------ |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`         |
| :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2]` |
- `Indeterminate` returns checked items, and always returns a parent when one or more children are selected.
| Selection                                                                                                                | Final Value                |
| ------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1, parent]`         |
| :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2, parent]` |
- `Exclusive` returns either the parent or child elements, but not both.
| Selection                                                                                                                | Final Value        |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------ |
| :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[parent]`         |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`         |
| :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`     | `[child1, child2]` |

### Radio Buttons

![A radio button form input with different options to select](/img/d9cb554c-d24b-44b5-a0a1-f28187542f34.webp)

Radio button input that allows users to select a single value from multiple choices.

| Configuration | Options                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------ |
| Types         | `String`, `Integer`, `Big Integer`, `Float`                                                                        |
| Choices       | Options for the radio buttons. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                        |

## Relational

### File

Upload a single file of any mime-type, choose an existing file from the [file library](/guides/files/manage), or import a file from a URL.

| Configuration | Options                                                                        |
| ------------- | ------------------------------------------------------------------------------ |
| Types         | `UUID`                                                                         |
| Root Folder   | Folder for the uploaded files. Does not affect the location of existing files. |

### Image

Upload a single image file, choose an existing image from the [file library](/guides/files/manage), or import an image from a URL.

| Configuration | Options                                                                        |
| ------------- | ------------------------------------------------------------------------------ |
| Types         | `UUID`                                                                         |
| Root Folder   | Folder for the uploaded files. Does not affect the location of existing files. |
| Crop to Fit   | Crop the image as needed when displaying the image.                            |

### Files

Upload multiple files, choose existing files from the [file library](/guides/files/manage), or import files from URL.

When this field is added to a collection, Directus will create a Many to Many (M2M) junction collection on your behalf.

| Configuration    | Options                                                                                                                      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Types            | `Alias`                                                                                                                      |
| Folder           | Folder for the uploaded files. Does not affect the location of existing files.                                               |
| Display Template | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |

### Builder (M2A)

Create relationships between the current item and multiple items from any number of other collections in a many-to-any (M2A) interface.

When this field is added to a collection, Directus will create a Many to Many (M2M) junction collection on your behalf.

| Configuration       | Options                                          |
| ------------------- | ------------------------------------------------ |
| Types               | `Alias`                                          |
| Related Collections | Which collections should items be selected from. |
| Allow Duplicates    | Allow users to add the same Item multiple times. |

Read our tutorial on using a Builder (M2A) to create reusable page components.

### Many To Many

Create relationships between the current item and many different items from a single collection.

When this field is added to a collection, Directus will create a Many to Many (M2M) junction collection on your behalf.

| Configuration            | Options                                                                   |
| ------------------------ | ------------------------------------------------------------------------- |
| Types                    | `Alias`                                                                   |
| Related Collection       | Which collection should items be selected from.                           |
| Layout                   | How to present items in the editor. `List`, `Table`                       |
| Junction Fields Location | Where in the editor the relational field should be added. `Top`, `Bottom` |
| Allow Duplicates         | Allow users to add the same Item multiple times.                          |
| Filter                   | Filter the list of items a user can select.                               |
| Item link                | Toggle visible link to the item.                                          |

### One to Many

Create a relationship between the current item and many items from a single collection.

When this field is added to a collection, Directus will create a corresponding Many to One field on the related collection on your behalf.

| Configuration      | Options                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| Types              | `Alias`                                                                                                                      |
| Related Collection | Which collection should items be selected from.                                                                              |
| Foreign Key        | Name of field in related collection to be created.                                                                           |
| Layout             | How to present items in the editor. `List`, `Table`                                                                          |
| Display Template   | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter             | Filter the list of items a user can select.                                                                                  |
| Item link          | Toggle visible link to the item.                                                                                             |

### Tree View

A special One to Many interface that allows users to create and manage recursive relationships between items from the same collection.

The Tree View interface is only available on self-referencing (recursive) relationships.

| Configuration      | Options                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| Types              | `Alias`                                                                                                                      |
| Related Collection | Which collection should items be selected from.                                                                              |
| Foreign Key        | Name of field in related collection to be created.                                                                           |
| Display Template   | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter             | Filter the list of items a user can select.                                                                                  |

### Many to One

Create a relationship between the current item and a single item from single collection.

| Configuration      | Options                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| Types              | `Alias`                                                                                                                      |
| Related Collection | Which collection should items be selected from.                                                                              |
| Display Template   | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter             | Filter the list of items a user can select.                                                                                  |

### Translations

Special relational interface designed specifically to handle translations. When this field is added to a collection a Languages Collection will be created by Directus on your behalf if it does not already exist.

| Configuration             | Options                                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------------------ |
| Types                     | `Alias`                                                                                                |
| Languages Collection      | Which collection to use for language selection.                                                        |
| Language Indicator Field  | The field from your `languages` collection that identifies the language to the user.                   |
| Language Direction Field  | The field from your `languages` collection that identifies the text direction for a selected language. |
| Default Language          | Default language to use.                                                                               |
| Use Current User Language | Default to the current users language.                                                                 |

## Presentation

### Divider

A horizontal divider to separate fields into different sections.

### Button Links

Group of one or more buttons that link to a preset or dynamic URL. Each link has the following configuration:

| Configuration | Options                                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Label         | Label for the button.                                                                                                     |
| Icon          | Icon displayed beside the button label.                                                                                   |
| Type          | The colors used by the button. `Primary`, `Normal`, `Info`, `Success`, `Warning`, `Danger`                                |
| URL           | URL to send the user to when the button is clicked. You can use field values from the item to create the URL dynamically. |

### Notice

An alert or notice interface to notify users of important information.

## Groups

### Accordion

User-controlled showing and hiding of fields within the group by clicking on each field.

| Configuration  | Options                                                |
| -------------- | ------------------------------------------------------ |
| Accordion Mode | Toggle whether only one section can be open at a time. |
| Start          | `All Closed`, `First Opened`                           |

### Detail Group

User-controlled showing and hiding of sub-groups by clicking on the group header.

| Configuration | Options |
| ------------- | ------- |
| Types         | `Alias` |

### Raw Group

Interface that groups multiple fields together in the data model, with no visual distinction.

| Configuration | Options |
| ------------- | ------- |
| Types         | `Alias` |

## Other

### Collection Item Dropdown

Dropdown to select an item from an existing collection.

| Configuration    | Options                                                                                                                      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Types            | `JSON`                                                                                                                       |
| Collection       | Which collection should items be selected from.                                                                              |
| Display Template | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter           | Filter the list of items a user can select.                                                                                  |

### Hash

Text input that allows users to hash the value on save. The Directus APIs can be used to [verify the hash](/api/utilities#verify-a-hash).

| Configuration | Options                                  |
| ------------- | ---------------------------------------- |
| Types         | `Hash`                                   |
| Masked        | Toggle raw value visibility before save. |

### Slider

Range input that allows users to select a number with an interactive slider.

| Configuration     | Options                                                             |
| ----------------- | ------------------------------------------------------------------- |
| Types             | `Integer`, `Decimal`, `Big Integer`, `Float`                        |
| Minimum Value     | Minimum value that can be set by the user.                          |
| Maximum Value     | Maximum value that can be set by the user.                          |
| Step Interval     | Specify the size of each increment (or step) of the slider control. |
| Always show value | Toggle visibility of the numeric value below the slider.            |


# Relationships

## Description

Leverage Directus relationships to create powerful data connections. Explore Many to One, One to Many, Many to Many, and Many to Any relationships, along with Translations for multilingual content management.

Directus supports all standard relationship types, as well as a few more of its own compound types, meant to streamline certain common configurations.

## Many to One (M2O)

In a M2O relationship, multiple items from the one collection are linked to one item in a different collection. One field is added to the 'many' collection referencing the primary key of the 'one' collection.

In :product-link{product="editor"}, having a M2O field does not automatically provide navigation to the related collection's items. To achieve this, the related collection requires a One to Many field to be set up.

**M2O Examples**
- Given `cities` and `countries` collections, many cities would be assigned to one country.
- Given `orders` and `customers`, many orders would be assigned to one customer.
- Given `books` and `publishers`, many books would be assigned to one publisher.

## One to Many (O2M)

In a O2M relationship, one item from a collection is linked to multiple items in a different collection.

Creating a O2M interface on the 'many' side of a M2O relationship creates an `Alias` field, which lets us access related items. This does not create a new database column as the O2M field is purely virtual. It creates an interface within the :product-link{product="editor"} to access items from an O2M perspective.

## Many to Many (M2M)

In a M2M relationship, an additional collection is created known as the junction collection. The junction collection stores the primary keys from two related collections, allowing for any number of items to be related between two collections.

You can also have a self-referencing M2M relationship that connects items in the same collection.

**Self-Referencing M2M Examples**
- Given an `articles` collection, you could configure related articles.
- Given a `users` collection, you could configure a friends list.
- Given `papers`, you could configure citations.

One example is "Related Articles", where each article relates to many other articles.

## Many to Any (M2A)

In a M2A relationship, one collection can be related to any item in any collection. This is sometimes known as a matrix field or replicator.

When you configure a M2A in Directus, a M2A `Alias` field is created as well as a junction collection. The junction collection in a M2A relationship also stores the collection name for related collections.

Read our tutorial on using a Builder (M2A) to create reusable page components.

## Translations

When you create a Translations interface in Directus, a translations O2M `Alias` field is created, as well as a `languages` collection and a junction collection between your main collection and `languages`. All translated text is stored in the junction collection.


# Collection Explorer

## Description

Learn to filter, layout, batch edit and more with collections in the collection explorer.

The content module allows users to browse, filter, and search for items held in collections. When users navigate into viewing single items, they use :product-link{product="editor"}. Each page contains data from a single collection, but can display related fields for each item.

To open the collection explorer, click on the content module on the left hand side of the page.

## Filtering Items

You can create custom filters to display items that fulfill certain criteria.

Click on :icon{name="material-symbols:filter-list"} at the top of the page to create a filter. You can then select a field to filter by and click on the criterion to tweak what should pass for that item to be filtered.

### And / Or Groups

`AND` groups give the option to filter for items that meet all of several criteria. On the other hand, `OR` groups filter for items that meet any one of several criteria.

In order for filters to be included in `AND` or `OR` groups, filters must be indented below them in the filter UI.

### Dynamic Variables

The following dynamic variables are built into Directus to add extra functionality to filters: 
- **`$CURRENT_USER`** — The primary key of the currently authenticated user.
- **`$CURRENT_ROLE`** — The primary key of the role for the currently authenticated user.
- **`$CURRENT_ROLES`** - An array of roles containing the `$CURRENT_ROLE` and any roles included within it.
- **`$CURRENT_POLICIES`** - An array of policies assigned to the user directly, or through their roles.
- **`$NOW`** — The current timestamp.
- **`$NOW(<adjustment>)`** - The current timestamp plus/minus a given distance, for example `$NOW(-1 year)`, `$NOW(+2 hours)`.

## Layouts

Layouts are customized mechanisms for viewing and interacting with the items in a collection. You can [select a layout](/guides/content/layouts) for displaying your collection. Note that restrictions will apply depending on your collection's [data model](/guides/data-model/collections).

## Batch Editing

By selecting more than one item in the explorer will allow you to click on :icon{name="material-symbols:filter-list"} and edit several items' fields at once to have the same value.

## Bookmarks

Bookmarks are custom views for your collections that include specified configurations, layouts, visible fields, sorting, filtering and more.

To create a bookmark, navigate to the Settings -> Bookmarks module. Here, you can create a new one by clicking on :icon{name="material-symbols:add-circle-outline-rounded"}.

You'll see the "Editing Preset" form, where you can set the name and collection, amongst layout and other values for this bookmark. Note that leaving the name field empty will make it so this bookmark is what is viewed for this collection by default.

Each Collection Page displays all Items in its Collection and comes with highly configurable Layouts for browsing, visualizing, and managing Items. The Page Header includes key action buttons for sorting, searching, filtering, creating, editing, archiving, and deleting multiple Items. To learn more, see our guide on the Collection Page.

The Content Module consists of :product-link{product="explore"}, where multiple items from a collection are displayed, and :product-link{product="editor"}, where single items can be displayed and edited.

A powerful, yet extensible, way to explore your database. Suitable for everyone in your organization with a robust permissions system.

Filters & Search: Filter your data with our powerful query builder across just one or related collections.

Layouts: Layouts are customized displays for viewing and interacting with the Items in a Collection. This makes working with specific types of data models, such as map locations or calendar events, a more human-friendly experience.

Save layout presets: Save your data layouts, filters, and sorts in presets and make them available to specific users or roles.


# Item Editor

## Description

Learn to create, duplicate, archive and perform other actions with items using Directus.

The item editor is a tailored form for managing individual items and their field values.

## Fields & Data Model

You can add fields to items by [configuring the collection's data model](/guides/data-model/fields). Here, you can also configure how the fields are displayed in the item editor.

## Creating Items

To create an item, click :icon{name="material-symbols:add-circle-outline-rounded"} in the page header to open the item page.

Fill in the fields as desired. Note that some of these will be [marked as required](/guides/data-model/fields) and need to be filled in, or be dynamic fields. Relations will be filled in here, too.

**Singletons**  
If the collection is configured as a [singleton](/guides/data-model/collections) in the data model
settings, the App will automatically open the item page when selecting the collection.

## Duplicating Items

When editing an item, you can click on :icon{name="material-symbols:more-vert"} to select some advanced options, amongst them "Save as Copy". Selecting this will save a copy.

## Archiving Items

To archive an item, follow these steps, navigate to the content module and select the desired collection. Select the desired item to open the item editor. Click :icon{name="material-symbols:archive"} located in the header and a popup will appear to confirm the action.

Archived items will not show up in app, but will still be returned in API responses unless explicitly filtered out.

**Requires Configuration**  
Archiving requires an [archive field](/guides/data-model/collections) to be configured within the collection's data model
settings.

## Revisions

As you update field values on items, Directus saves these revisions, and they can be compared side-by-side to the current state.

To revert an item, navigate to the content module and select the desired collection and select the desired item. Click on "Revisions" in the editor sidebar and then on the revision you wish to preview. Go to "Revisions Made" in the side menu and view the revision differences. Click :icon{name="material-symbols:settings-backup-restore"} to revert the item's values and return to the item page.

**Revision Preview**  
You will also see a "Revision Preview" button in the side menu navigation, which will let you preview all the item's
values for that revision.

You can also revert items [programmatically via the API](/api/revisions).

## Comments

You can add comments to items in the sidebar by clicking on "Comments", which will show the form for submitting one. You can use the @ button to tag specific users in your comment.

## Shares

You can create shareable links to view an item in the sidebar by clicking on Shares -> New Share.

Here, you can specify the name, password, roles allowed to access the item, as well as the start and end dates for the link's validity, followed by the maximum times a link can be used.

To share the link, click on the new share's :icon{name="material-symbols:more-horiz"} and select either "Copy Link" or "Send Link". You can also edit or destroy the share in this menu.

## Next Steps

Learn how to use [content versioning](/guides/content/content-versioning) and the [live preview](/guides/content/live-preview) functionality.


# Layouts

## Description

Learn to use layouts for viewing and interacting with items in a collection using Directus.

Layouts are customized mechanisms for viewing and interacting with the items in a collection.

## Adjust a Collection's Layout

To adjust an item's layout, navigate to the content and select the collection you wish to work with. In the page sidebar, click on "Layout Options". Then you can choose the desired layout type you want to use and customize it accordingly.

Layouts are tailored to work with specific data-models. For example, in order to work properly, the map layout requires
the collection have a map field configured and the calendar layout requires the collection have a datetime field configured.

Each layout presents data differently, so certain customizations may not be functional with certain layouts. For example,
the map layout displays each item as a pin on a map, so this layout has no controls for sorting.

Depending on the layout, the same control may be under layout options in the sidebar, the subheader, or on the page area
(and items themselves). For example, the table layout lets you set the field values displayed in the subheader while
the card layout lets you set field values displayed in the layout options menu.

### Customization Controls

Customization controls can be found in the following three locations:
- **Layout Options** — Located in the sidebar.
- **Subheader** — Located just below the page header and above the page area.
- **Page Area** — The center of the webpage, which displays all items.

These controls typically fall into three general categories.

| Category       | Description                                                                                       |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Styling and Formatting       | These are additional customizations to the way a layout displays such as the size of each Item, how images are cropped, etc.                                                                                       |
| Field Values Displayed       | Most layouts allow you to choose which field value(s) are used to represent each item on the collection page. For example, with blog posts, it may be ideal to have the hero image, blog title, date, author, etc. |
| Manual and Automatic Sorting | Certain layouts may allow sorting items by value in ascending and descending order, drag-and-drop repositioning of items, etc.                                                                                     |

## Table Layout

This layout displays items in a tabular form, making it compatible with all kinds of items. This is the default
layout used in the content module.

### Layout Options

| Control         | Description                               |
| --------------- | ----------------------------------------- |
| **Spacing** | Adjust the vertical space a row takes up. |

### Subheader

| Control                          | Description                                                                                                             |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Adjust Column Width**      | Click and drag the column divider to resize as desired.                                                                 |
| **Add Field**                | Select :icon{name="material-symbols:add-circle-outline-rounded"} in the page subheader and select the desired Field(s). |
| **Remove Field**             | Select :icon{name="material-symbols:arrow-drop-down-circle"} in the column title and click **"Hide Field"**.        |
| **Sort Items by Column**     | Select :icon{name="material-symbols:arrow-drop-down-circle"} in the column title and sort ascending or descending.      |
| **Set Text Alignment**       | Select :icon{name="material-symbols:arrow-drop-down-circle"} in the column title and set left, right, or center.        |
| **Toggle & Reorder Columns** | Click the column header, then drag-and-drop as desired.                                                                 |
| **Select All**               | Click :icon{name="material-symbols:check-box-outline"} in the selection column header.                                  |

### Page Area

| Control                     | Description                                                                                                                                              |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Select Item(s)**      | Click :icon{name="material-symbols:check-box-outline"} in the selection column for the desired Item(s).                                                  |
| **Manually Sort Items** | Toggle :icon{name="material-symbols:check-box-outline"} in the configured Sort column to drag and drop :icon{name="material-symbols:drag-handle"} Items. |

**Manual Sorting Requires Configuration**  
Only available if you [configure a sort field](/guides/data-model/collections) in the collection's data model
settings.

## Card Layout

This tiled layout is ideal for collections that prioritize an image. This is the default
for both the user directory and
file library. It includes the following controls.

### Layout Options

| Control | Description |
|---|---|
| **Image Source** | Set the field used as the display image. |
| **Title** | Sets a display template to use as a title. |
| **Subtitle** | Sets a display template to use as a subtitle. |
| **Image Fit** | Set how an image fits inside the card. |
| **Fallback Icon** | Set a default icon to display when an item has no image. |

### Subheader

| Control | Description |
|---|---|
| **Card Size** | Toggle the card size as it appears in the page area. |
| **Order Field** | Click to select the field you wish to order by from the dropdown menu. |
| **Order Direction** | Toggle ascending and descending order. |
| **Select All** | Click  ":icon{name="material-symbols:check-circle"} Select All" in the selection column header. |

### Page Area

| Control | Description |
|---|---|
| **Select Item(s)** | Click ":icon{name="material-symbols:radio-button-unchecked"} in the selection column for the desired item(s). |


## Calendar Layout

This layout is ideal for collections with time-oriented data (e.g. events and appointments).

### Layout Options

| Control | Description |
|---|---|
| **Display Template** | Set a mix of field values and text to represent items on the calendar. |
| **Start Date Field** | Choose field to determine each item's beginning time on the calendar. |
| **End Date Field** | Choose field to determine each item's ending time on the calendar. |
| **First Day of The Week** | Defines the beginning of the week on the calendar.  |

### Subheader

| Control | Description |
|---|---|
| **Toggle Month and Year** | Move across time using the chevrons in the subheader. |
| **Today** | Click to jump to the current date on the calendar. |
| **Month Week Day List** | Adjust the time interval used to display items in the page area. |

### Page Area

| Control | Description |
|---|---|
| **Select Item** | Click an item on the calendar to open its item page. |

**Configuration Requirements**  
To use this layout, the collection will need at least one datetime [Field](/guides/data-model/fields) to set a start time,
but ideally two datetime Fields (to set a start time and end time).

## Map Layout

This layout is ideal for collections that emphasize geospatial data. It provides a world map, with items displayed as
points, lines, and other geometry.

### Layout Options

| Control | Description |
|---|---|
| **Basemap** | Choose the map provider used for the collection. |
| **Geospatial Field** | Select the geospatial field type to display on screen: <ul><li>Map JSON Point: Supports latitude-longitude based, single-point locations.</li><li>Map Geometry: Supports geometric areas such as lines and polygons.</li></ul> |
| **Display Template** | Choose the fields displayed when hovering over an item on the map. |
| **Cluster Nearby Data** | Toggle whether or not nearby items get clustered into a single pin.  |

### Subheader

There is no Subheader on the Map Layout.

### Page Area

| Control | Description |
|---|---|
| **Zoom** | Click :icon{name="material-symbols:add"} and :icon{name="material-symbols:remove"} in the upper left hand corner of the page area to zoom in and out. |
| **Find my Location** | Click :icon{name="material-symbols:my-location"} to zoom into your current location on the map. |
| **Reframe** | Click the square in the upper left-hand corner to resize and reframe the map area. |
| **Select Item** | Click a single item to enter its item page. |
| **Select Items** | Click and drag to select multiple items at once, opening the item page. |

**Configuration Requirements**  
To use this Layout, the collection must have a map field configured.

## Kanban Layout

This layout is ideal for collections that serve as project management tools or to-do lists, where each item represents a
task, because it groups items onto columns according to their status (e.g. "Not Started", "In Progress", "Under
Review", "Complete", or any other status defined).

### Layout Options

| Control | Description |
|---|---|
| **Group By** | Select the field used to define item status. See below for details. |
| **Card Title** | Choose the field use to serve as the title for each kanban board. |
| **Card Text** | Choose a field to display additional text on each item.  |

### Layout Options > Advanced

| Control | Description |
|---|---|
| **Card Tags** | Choose a tag field to be displayed on the item. |
| **Card Date** | Choose a datetime field to be displayed on each item. |
| **Card Image** | Choose an image field to be displayed on each item. |
| **Card Image Fit** | Toggle whether the image fit is cropped. |
| **Card User** | Choose the user created field to display their avatar in the bottom right corner. |
| **Show Ungrouped** | Toggle display of a column containing Items with no assigned status.  |

### Subheader

There is no Subheader for the Kanban Layout.

### Page Area

| Control | Description |
|---|---|
| **Create Task and Assign Status** | Click :icon{name="material-symbols:add"} in a status column and the item page will open. |
| **Sort Panels** | Drag and drop items to reposition or change task status. |
| **Add Status Panel**  | Click :icon{name="material-symbols:add-box"} and add a group name (i.e. new status column). |
| **Edit or Delete Status Column** | Click :icon{name="material-symbols:more-horiz"} and then click :icon{name="material-symbols:edit"} to edit or :icon{name="material-symbols:delete"} to delete. |

**Configuration Requirements**  
To make this layout work, you will need to configure an appropriate status [field](/guides/data-model/fields) on the
collection, then identify this field under "Group By" in the Layout Options menu.
