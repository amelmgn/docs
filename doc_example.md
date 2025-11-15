
---
title: Collections
description: Directus collections help you manage your data. Create custom collections, define fields, configure relationships, and leverage features like content versioning and access control. Learn how to create, configure, and manage collections in Directus.
---

Collections are database tables with additional metadata and configuration used by Directus.

All Directus-specific settings and data is held only in system collections prefixed with `directus_`. Should you wish to remove Directus, you can remove these collections with no proprietary data left behind.

There are two types of collections: **user collections** and **system collections**.

## Creating Collections

**User collections** are created directly in your database or via Directus. They describe your specific data models and configurations and can be queried via APIs created by :product-link{product="connect"} or via :product-link{product="explore"}.

You can create collections from the Data Model settings in the Directus Data Studio, or using the [Collections API](/api/collections).

![Creating a collection](/img/2e088221-6bc5-4c00-b348-e23f77a9a748.webp)

### Name

The unique name for your collection must be set on creation, is case sensitive, and will be used throughout the Data Studio and Directus APIs.

::callout{icon="material-symbols:warning-rounded" color="warning"}
**Collection names are immutable**
The collection name cannot be modified after collection creation, which includes casing. However, you can override how it is displayed in the Data Studio with collection naming translations.
::

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

::callout{icon="material-symbols:code-blocks-rounded" color="green" to="/api/collections"}
Explore the Collections API to create and manage collections.
::

## Configuring Collections

Once a collection is created, there are a number of configuration options available.

### Collection Setup

![Configuration settings for the posts collection](/img/5b85f21c-b96f-4453-887b-c043f167b523.webp)

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

::callout{icon="material-symbols:menu-book-outline" color="primary" to="/guides/content/content-versioning"}
Read the content versioning user guide.
::

### Live Preview

Live preview is used by :product-link{product="editor"} and allows for your application to be shown in a pane next to your content, which can be used for previewing content before publishing.

You can use item field values to construct the URL used by the live preview, including unique identifiers and content version, allowing for previewing content versions before promoting them to the main version.

::callout{icon="material-symbols:school-outline" color="secondary" to="/tutorials/getting-started"}
Read tutorials on implementing live preview.
::

### Accountability

**Revisions** are created whenever an item is updated. This history of versions is tracked so that previous states can be recovered. Every change made to items in Directus is stored as a complete versioned snapshot and a set of specific changes made.

**Activity** is a log of all events that take place within the platform. Each activity record tracks the event type, user, timestamp, IP address, user-agent, and any associated revision data.

By default, your Directus project tracks all activity and revisions for collections. However, you can override this and choose to only track activity, or nothing.

::callout{icon="material-symbols:info-outline"}
**Accountability vs Telemetry**
Accountability is a log of who does what in your project. It is for your team's own use. This is different from telemetry, which is configured in environment variables.
::

### Sorting

The sort feature enables users to manually sort items in the Data Studio and via the API. If not selected as an optional field when creating a collection, you will need to create a new field with an integer type. It can then be selected as the sort field.

::callout{icon="material-symbols:info-outline"}
**Sorting Relational Fields**
To configure manual sorting within a [relational field](/guides/data-model/relationships) (e.g., M2M, O2M, or M2A), also set the sort field within the relationship section of the field's configuration drawer.
::

Once configured, click :icon{name="material-symbols:sort" title="Sort Button"} in the configured sort column, and then drag items by their :icon{name="material-symbols:drag-handle" title="Drag Button"} handle. You can also sort by the sort field with querying data via the [Items API](/api/items).

### Duplication

The **Save as Copy** option in :product-link{product="editor"} offers a way to effectively duplicate the current item. Duplication settings define which field values will be copied.

::callout{icon="material-symbols:info-outline"}

**Duplicating Items with Relationships**<br/>

When you duplicate an item, any related items are not copied. You must create new relationships between the duplicated item and related items.

::

### Archive Settings

:video-embed{video-id="1fb83779-28e9-4523-bcbc-57065d7177a1"}

Archived items still exist in your collections, but are filtered within the Data Studio. If not selected as an optional field when creating a collection, you will need to create a new field.

To configure an archive field, set the following four input fields as desired.

- **Archive Field**: selects the archive field.
- **Archive App Filter**: can users filter for archived items.
- **Archive Value**: assigned value to archive field when an item is archived.
- **Unarchive Value**: assigned value to archive field when an item is unarchived.

::callout{icon="material-symbols:info-outline"}
**Archived Items via API**
Archived items are hidden in the app by default, but they are still returned normally via the API unless explicitly filtered out. This gives you the flexibility to manage archived items however you want when working with the API.
::

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

![System database tables](/img/426fb648-1e88-46e4-92f1-af76f3254d25.webp)

When Directus is connected to an existing database, it will introspect existing tables and relationships and collections will be made available to admins via the [Items API](/api/items).

To access the collection via the Data Studio, you must configure the collection with additional Directus-specific metadata, such as interfaces, displays, and icons.

To access the collection without an admin role, you must also configure [access control](/guides/auth/access-control) as with any other collection.

Finally, any relationships not configured in the database must also be set up inside of Directus.

::callout{icon="material-symbols:info-outline"}
**Composite Keys & SQL Views**
Directus does not currently support composite keys, or the creation of virtual tables via SQL views.
::

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Fields
description: Directus fields let you define how your data is stored and displayed. Learn about creating fields, data types, interfaces, validations, relationships, and more. Discover how to configure fields to perfectly suit your data modeling needs in Directus.
---

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

![Field creation form showing datetime field](/img/426fb648-1e88-46e4-92f1-af76f3254d25.webp)

The **interface** describes how users will create and edit data, as well as how it is displayed in :product-link{product="editor"}. There are many kinds of built-in interface, such as a text input, date selector, map, and a set of relationship interfaces. More interfaces can be built as [extensions](/guides/extensions/overview).

The **key** is the unique name for a field within a collection. This value is used in both the Data Studio and via API. Within the Data Studio, the key is parsed through our title formatter to improve readability.

The **type** defines the underlying data type configured in the database. When creating fields, the available types will correspond to the interface selected.

::callout{icon="material-symbols:warning-rounded" color="warning"}
**Field names and types are immutable**
The field key and type cannot be modified after collection creation, which includes casing. However, you can override how the key is displayed in the Data Studio with field name translations.
::

![Field configuration form showing datetime field](/img/1234cdf2-778e-4e3a-836c-bb698398848b.webp)

## Configuring Fields

There are a number of advanced configuration options available for fields. Some must be configured at the time of creation, while others can be edited after creation.

### Schema

This section defines the database column settings for the field. This includes the key, type, length (if applicable), default value, whether the field is nullable, whether values must be unique in the collection, whether the field is indexed, and whether or not to include the field in searches.

### Field

This section sets details for the interface, which is displayed in the editor. This includes whether the field is read-only or requires a value to be provided when an item is created.

This section also allows for a note - which is displayed in the editor and provides additional context or help to the user, and field name translations, which changes the label of this field in the editor based on the selected language in the Data Studio.

### Interface

This section describes how the user will create, edit, and view data in the editor, and are effectively different form inputs. Each interface will have its own configuration options.

::callout{icon="material-symbols:menu-book-outline" color="primary" to="/guides/data-model/interfaces"}
See all available interfaces in Directus.
::

### Display

This section describes how individual field values will be displayed throughout the Data Studio. You can always choose to display the raw value, but you can display a formatted value instead. Each interface will have its own display options.

Conditional styles allow for different colors, icons, and text values to be used when the field data meets configured criteria.

### Validation

This section allows for the configuration of rules to determine valid user input. If validation fails, you can also use this configuration section to write custom messages.

These validations apply to data added via Directus and are not database-level validations.

### Conditions

This section alters the current field's configuration based on the values of other fields in the item. Conditional configuration include whether a field is read-only, hidden, or required, along with any editable interface-specific configuration.

::callout{icon="material-symbols:info-outline"}
**Conditionally Show Fields**
To show a field conditionally, set hidden to `true` in the field settings. Then, create a condition with a rule checking the value of another field. The condition should set hidden to `false`.
::

### Relationships & Translations

This section will only appear when the field is relational, and describes the relationship between this field and other collections. Each relationship type will be shown differently, and where there is a junction collection you will be given the option to add a sort field on the collection.

Relational triggers configure what will happen to the relational field data when related values are deselected or deleted. This includes setting values to `null`, their default, or cascading/preventing the deletion.

::callout{icon="material-symbols:menu-book-outline" color="primary" to="/guides/data-model/relationships"}
See all available relationship types in Directus.
::

### Field Width

Each field can be configured to be half or full width in the editor. Two half-width fields can be placed next to each other. Additionally, a field can be configured to use up the full-width of the editor, expanding beyond the usual full-width container.

Field width is not in the field configuration, but can be set by clicking on :icon{name="material-symbols:more-vert" title="Field Width Button"} in the collection data model page.

::callout{icon="material-symbols:info-outline"}
**Field Width Configurations**

- `half` fields will go from `[start]` until `[half]` in the grid system, but have a maximum width of `380px` defined by the `--form-column-max-width` css variable.

- `full` fields will be the sum of 2 `half` fields, with a maximum width of `760px`.

- `fill` fields will fill the available space, without any maximum.

::

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

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Interfaces
description: Manage your data effectively with Directus fields. Discover various field types, interfaces, validations, and relationships to perfectly suit your data modeling needs.
---

Interfaces are how users interact with fields in :product-link{product="editor"}. Each interface supports specific data types and configurations.

## Text & Numbers

### Input

![A standard form text input](/img/02d2521b-8e5a-4a2f-8663-729c77f00493.webp)

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

![An autocomplete form text input that shows a dropdown list of options based on a search query](/img/02d2521b-8e5a-4a2f-8663-729c77f00493.webp)

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

::callout{icon="material-symbols:info-outline"}
**Throttle vs Debounce**
Throttle and debounce are very similar. Debounce will wait until a period of 'silence' has happened before making the request, while throttle will keep making requests at most 1 call every period. Period is set in the 'rate' configuration for this interface.
::

### Block Editor

![Showcase of the block editor with example blocks](/img/0344b52f-aa52-4a57-9f8c-2a9c4fa0b10e.webp)

Allows users to create and edit content using blocks. These blocks represent individual pieces of content, such as text,
images, videos, buttons, and more, that can be assembled and re-arranged within a flexible layout.

| Configuration | Options                                                                 |
| ------------- | ----------------------------------------------------------------------- |
| Types         | `JSON`                                                                  |
| Toolbar       | Allows for customization of visible formatting options.                 |
| Root Folder   | Default folder to store uploaded files. Does not affect existing files. |

### Code

![A code editor input](/img/a07b7361-671f-48ca-944f-acefa2bbe1e1.webp)

Code editor for pre-formatted text.

| Configuration | Options                                                                                       |
| ------------- | --------------------------------------------------------------------------------------------- |
| Types         | `String`, `Text`, `JSON`, `Geometry (All)`                                                    |
| Language      | Language for syntax highlighting. Can be set when type is not `JSON`.                         |
| Line Number   | Toggle to show line numbers.                                                                  |
| Line Wrapping | Toggle to wrap text to prevent horizontal scrolling.                                          |
| Template      | Preset value that the user can add to the field value by clicking "Fill with Template Value". |

### Textarea

![A standard form textarea input](/img/1b66cfc0-785d-40ed-adf3-9ce90441142b.webp)

Textarea input for longer plain text.

| Configuration | Options                                                        |
| ------------- | -------------------------------------------------------------- |
| Types         | `Text`                                                         |
| Soft Limit    | Used to limit the number of characters within the Data Studio. |

### WYSIWYG

![A What You See Is What You Get (WYSIWYG) form input that has a toolbar for formatting](/img/262f71a7-8cc8-4402-85b9-b80dbc2a60ba.webp)

The What You See Is What You Get (WYSIWYG) editor provides a text area with rich formatting options in the toolbar.

| Configuration       | Options                                                                                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Types               | `Text`                                                                                                                                                                                |
| Toolbar             | Allows for customization of visible formatting options                                                                                                                                |
| Folder              | Default folder to store uploaded files. Does not affect existing files.                                                                                                               |
| Soft Limit          | Used to limit the number of characters within the Data Studio.                                                                                                                        |
| Static Access Token | Token appended to asset URLs when displaying in the editor.                                                                                                                           |
| Custom Formats      | JSON array of formatting that is passed to the `style_formats` config option of the WYSIWYG editor instance. [TinyMCE Documentation](https://www.tiny.cloud/docs/demo/format-custom/) |
| Options Override    | JSON object to override the default config option of the WYSIWYG editor instance. [TinyMCE Documentation](https://www.tiny.cloud/docs/configure/)                                     |

### Markdown

![A markdown text editor with a toolbar with formatting options. Edit and preview tabs.](/img/b41d6822-35ab-48db-ada9-dd3a723a5c52.webp)

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

![A standard form text input where user can select, add, and remove tags.](/img/510231f2-4336-4071-bf41-7c9d4193cca3.webp)

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

![A toggle form input with label named "Enabled"](/img/532c82dd-c851-4874-b596-597421dfc3cd.webp)

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

![A date picker input. User can select a calendar date and input a time. ](/img/b5ba6fb2-0e24-408a-b4c5-78c898558293.webp)

Date picker input that allows user to select a date and time.

| Configuration      | Options                                 |
| ------------------ | --------------------------------------- |
| Types              | `DateTime`, `Date`, `Time`, `Timestamp` |
| Include Seconds    | Show seconds in the interface.          |
| Use 24-Hour Format | Use 24 hour time system.                |

::callout{icon="material-symbols:info-outline"}
**Handling Timezones**
The `DateTime` type does not have timezone information, and will always store values as UTC. The `Timestamp` type contains timezone information.
::

### Repeater

![A repeater field](/img/7bc91592-707a-4e61-8eca-667a1ecf8e8f.webp)

![A repeater field, open](/img/f2675394-689a-485f-9627-39992a97c30d.webp)

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

![An interactive map interface that shows a single point on the east coast of the United States. Map has buttons for zoom, search, and full screen.](/img/e8d96531-7a23-4516-af4d-4180b778ef84.webp)

Show and set geospatial data on an interactive map. Mapping data is stored as [GeoJSON](https://geojson.org/).

::callout{icon="material-symbols:info-outline"}
**Maps Provider**
By default, Directus will use [OpenStreetMap](https://www.openstreetmap.org) to display your mapping data.

[By entering a Mapbox API key](/configuration/general#mapbox), you can enhance the platform's mapping experience.
::

| Configuration | Options |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Types         | <ul><li>**`Point`**: A single location on a map.</li> <li>**`LineString`**: A series of points on a map connected in a line.</li> <li>**`Polygon`**: An area of a map drawn by selecting vertices.</li> <li>**`Multipoint`**: A series of disconnected points on a map.</li> <li>**`MultiLineString`**: A series of `LineString` objects.</li> <li>**`MultiPolygon`**: A series of `Polygon` objects.</li> <li>**`Geometry (All)`**: An series of `Point`, `LineString` and `Polygon` objects.</li> <li>**`JSON`**: A `Geometry (All)` object stored as JSON.</li> <li>**`String`**: A `Geometry (All)` object stored a string of characters.</li> <li>**`Text`**: A `Geometry (All)` object stored as Text.</li> <li>**`CSV`**: A `Geometry (All)` object stored as comma-separated values.</li> |
| Default View  | The default location and zoom settings on the map to show by default |
::callout{icon="material-symbols:info-outline"}
**Database Support**
Your database must support geospatial data or have a geospatial plugin installed, such as PostGIS or SpatiaLite, to fully utilize mapping functionality with Directus.
::

### Color

![A text input for color hex codes that allows user to select color modes ](/img/6429c983-5a72-4e77-aa8e-df816a0348d0.webp)

A color picker interface that allows users to input color codes and convert between different color modes.

| Configuration | Options                                           |
| ------------- | ------------------------------------------------- |
| Types         | `String`                                          |
| Opacity       | Enable opacity values to be adjusted by the user. |
| Preset Colors | Preset colors that users can select.              |

### Dropdown

![A select input with a dropdown of options.](/img/370efb9d-83de-4830-a33c-7de3a2e6e246.webp)

Input that allows the user to select a value from a dropdown list of options.

| Configuration | Options                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| Types         | `String`, `Integer`, `Big Integer`, `Float`, `Decimal`                                                        |
| Choices       | Options for the dropdown. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                   |
| Allow None    | Allow user to deselect an option.                                                                             |

### Dropdown (Multiple)

![A select input where user can select multiple options from a dropdown.](/img/888f1cb3-b88c-4fd4-b7cf-c8c7b4ddbf9a.webp)

Input that allows user to select multiple values from a list of options.

| Configuration | Options                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| Types         | `JSON`, `CSV`                                                                                                 |
| Choices       | Options for the dropdown. Each option contains text that is displayed to the user and a value that is stored. |
| Allow Other   | Allow user to enter custom values other than preset values.                                                   |
| Allow None    | Allow user to deselect an option.                                                                             |

### Icon

![A select input with a dropdown grid of icon choices.](/img/e33d72e2-2f3c-4c18-8446-b40d07ecc1b8.webp)

Allow the user to select from a list of icons. Icons are from the [Google Material Symbols](https://fonts.google.com/icons) library and cannot be swapped for another library.

| Configuration | Options  |
| ------------- | -------- |
| Types         | `String` |

### Checkboxes

![A form input with multiple checkboxes.](/img/7a7d6ff7-8931-4a44-8ccf-1f3a7d0f04d1.webp)

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

![A form input with a nested tree of multiple parent and child checkboxes.](/img/3a4950bb-da45-4814-95a6-eff7ae14fab8.webp)

Nested tree of checkboxes that can be expanded or collapsed.

| Configuration   | Options                                                                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Types           | `JSON`, `CSV`                                                                                                                                 |
| Choices         | Options for the checkboxes. Each option contains text that is displayed to the user and a value that is stored, along with any child options. |
| Choice Children | Child checkboxes nested below the current choice.                                                                                             |
| Value Combining | Controls what value is stored when nested selections are made. `All`, `Branch`, `Leaf`, `Indeterminate`, `Exclusive`.                         |

::callout{icon="material-symbols:info-outline"}
  **Understanding Value Combining Options**
  In a Checkboxes (Tree) interface, checkboxes can exist within a parent checkbox. Value combining determines the final value when selecting items in a tree.
  - `All` returns all checked values.
    :::collapsible{name="examples" class="mt-2"}
      | Selection                                                                                                                | Final Value                |
      | ------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
      | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`                 |
      | :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2, parent]` |
    :::
  - `Branch` returns the top-most values that are selected.
    :::collapsible{name="examples" class="mt-2"}
    | Selection                                                                                                                | Final Value |
    | ------------------------------------------------------------------------------------------------------------------------ | ----------- |
    | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`  |
    | :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[parent]`  |
    :::
  - `Leaf` returns the deepest values that are selected
    :::collapsible{name="examples" class="mt-2"}
    | Selection                                                                                                                | Final Value        |
    | ------------------------------------------------------------------------------------------------------------------------ | ------------------ |
    | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`         |
    | :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2]` |
    :::
  - `Indeterminate` returns checked items, and always returns a parent when one or more children are selected.
    :::collapsible{name="examples" class="mt-2"}
    | Selection                                                                                                                | Final Value                |
    | ------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
    | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1, parent]`         |
    | :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`         | `[child1, child2, parent]` |
    :::
  - `Exclusive` returns either the parent or child elements, but not both.
    :::collapsible{name="examples" class="mt-2"}
    | Selection                                                                                                                | Final Value        |
    | ------------------------------------------------------------------------------------------------------------------------ | ------------------ |
    | :icon{name="material-symbols:check-box"} `parent`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[parent]`         |
    | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box-outline"} `child2` | `[child1]`         |
    | :icon{name="material-symbols:check-box-outline"} `parent`<br>&emsp; :icon{name="material-symbols:check-box"} `child1`<br>&emsp; :icon{name="material-symbols:check-box"} `child2`     | `[child1, child2]` |
    :::
::

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

![A file type form input where user can pick from three options: "Upload File From Device", "Choose Files from Library", "Import File from URL"](/img/b7582b34-f741-48e4-9095-64b2d0995cdf.webp)

Upload a single file of any mime-type, choose an existing file from the [file library](/guides/files/manage), or import a file from a URL.

| Configuration | Options                                                                        |
| ------------- | ------------------------------------------------------------------------------ |
| Types         | `UUID`                                                                         |
| Root Folder   | Folder for the uploaded files. Does not affect the location of existing files. |

### Image

![An image form input where user can pick from three options: "Upload File From Device", "Choose Files from Library", "Import File from URL"](/img/b673b63d-75e0-4763-96fb-47263da2193a.webp)

Upload a single image file, choose an existing image from the [file library](/guides/files/manage), or import an image from a URL.

| Configuration | Options                                                                        |
| ------------- | ------------------------------------------------------------------------------ |
| Types         | `UUID`                                                                         |
| Root Folder   | Folder for the uploaded files. Does not affect the location of existing files. |
| Crop to Fit   | Crop the image as needed when displaying the image.                            |

### Files

![A file type form input where user can select and upload multiple files.](/img/aaca7ac1-ae3d-458b-8b5b-e4747a236cf0.webp)

Upload multiple files, choose existing files from the [file library](/guides/files/manage), or import files from URL.

When this field is added to a collection, Directus will create a Many to Many (M2M) junction collection on your behalf.

| Configuration    | Options                                                                                                                      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Types            | `Alias`                                                                                                                      |
| Folder           | Folder for the uploaded files. Does not affect the location of existing files.                                               |
| Display Template | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |

### Builder (M2A)

![A form interface that allows users to create a relationship from the current item by selecting different items from multiple, distinct Collections.](/img/df84bd0b-cdba-4920-bc3c-493928b763a7.webp)

Create relationships between the current item and multiple items from any number of other collections in a many-to-any (M2A) interface.

When this field is added to a collection, Directus will create a Many to Many (M2M) junction collection on your behalf.

| Configuration       | Options                                          |
| ------------------- | ------------------------------------------------ |
| Types               | `Alias`                                          |
| Related Collections | Which collections should items be selected from. |
| Allow Duplicates    | Allow users to add the same Item multiple times. |

::callout{icon="material-symbols:school-outline" color="secondary" to="/tutorials/getting-started/create-reusable-blocks-with-many-to-any-relationships"}
Read our tutorial on using a Builder (M2A) to create reusable page components.
::

### Many To Many

![A form interface that allows users to select multiple different items from a single collection. Buttons for "Create New" and "Add Existing".](/img/010f9c29-a4b1-4463-bc72-7106f41ef4a8.webp)

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

![A form interface that allows users to select multiple items from a single collection. Buttons for "Create New" and "Add Existing".](/img/63dcee0a-9ee7-4328-bd6b-bd85cc4660c9.webp)

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

![A form interface that shows multiple parent and child items from the same collection. Buttons for "Create New" and "Add Existing".](/img/aa6b991e-7c5b-459d-ae9a-7e3f7ffc4fb1.webp)

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

![A form interface that allows a user to select a single item from a collection."](/img/202ad6d5-b5cd-4592-a9b7-f26c79702fae.webp)

Create a relationship between the current item and a single item from single collection.

| Configuration      | Options                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| Types              | `Alias`                                                                                                                      |
| Related Collection | Which collection should items be selected from.                                                                              |
| Display Template   | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter             | Filter the list of items a user can select.                                                                                  |

### Translations

![A form interface with two columns and two fields per column - "Title" and "Content". One column contains the English translation for each field. Second column contains the French translation for each field.](/img/6641341c-c9ba-483e-bd6f-17d8275abb21.webp)

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

![A horizontal divider between two form fields](/img/7eb81773-59d7-46cb-95cd-6b50d8ef4bdf.webp)

A horizontal divider to separate fields into different sections.

<!-- TODO: Document config options -->

### Button Links

![A group of two buttons. One primary button. One default button.](/img/c88411d7-c4a9-44e8-8a55-78e84de7dbd2.webp)

Group of one or more buttons that link to a preset or dynamic URL. Each link has the following configuration:

| Configuration | Options                                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Label         | Label for the button.                                                                                                     |
| Icon          | Icon displayed beside the button label.                                                                                   |
| Type          | The colors used by the button. `Primary`, `Normal`, `Info`, `Success`, `Warning`, `Danger`                                |
| URL           | URL to send the user to when the button is clicked. You can use field values from the item to create the URL dynamically. |

### Notice

![A standard warning notice in yellow with a hazard icon.](/img/eff79b29-a12a-46b3-b01a-a0a329064f76.webp)

An alert or notice interface to notify users of important information.

<!-- TODO: Document config options -->

## Groups

### Accordion

![An accordion interface that allows use to expand and collapse different fields.](/img/774bc56b-adb5-44a2-8e65-69e770321977.webp)

User-controlled showing and hiding of fields within the group by clicking on each field.

| Configuration  | Options                                                |
| -------------- | ------------------------------------------------------ |
| Accordion Mode | Toggle whether only one section can be open at a time. |
| Start          | `All Closed`, `First Opened`                           |

### Detail Group

![A group of form fields that are currently hidden behind a toggle.](/img/f1d62298-5cce-41b5-9655-1014b97a6aac.webp)

![A group of form fields that are currently visible but can be hidden behind a toggle.](/img/9eec605f-af7b-4c39-a65a-73ad7440ed0b.webp)

User-controlled showing and hiding of sub-groups by clicking on the group header.

| Configuration | Options |
| ------------- | ------- |
| Types         | `Alias` |

### Raw Group

![A group of form fields](/img/d03eef30-f983-4efb-aac3-ebf120294516.webp)

Interface that groups multiple fields together in the data model, with no visual distinction.

| Configuration | Options |
| ------------- | ------- |
| Types         | `Alias` |

## Other

### Collection Item Dropdown

![A collection item field](/img/89863252-f709-4ddb-97b6-d298b01b05e5.webp)

Dropdown to select an item from an existing collection.

| Configuration    | Options                                                                                                                      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Types            | `JSON`                                                                                                                       |
| Collection       | Which collection should items be selected from.                                                                              |
| Display Template | Display templates are used to represent an item in relationship fields - for example to show the value of the `Title` field. |
| Filter           | Filter the list of items a user can select.                                                                                  |

### Hash

![Form text input that shows the value is securely hashed.](/img/6b62272a-8a1e-4bed-be6d-3cf614bb2f48.webp)

Text input that allows users to hash the value on save. The Directus APIs can be used to [verify the hash](/api/utilities#verify-a-hash).

| Configuration | Options                                  |
| ------------- | ---------------------------------------- |
| Types         | `Hash`                                   |
| Masked        | Toggle raw value visibility before save. |

<!-- TODO: Document missing values, like placeholder and masking -->

### Slider

![A slider input](/img/4fd92002-5f86-4e61-91f9-1169df4d6268.webp)

Range input that allows users to select a number with an interactive slider.

| Configuration     | Options                                                             |
| ----------------- | ------------------------------------------------------------------- |
| Types             | `Integer`, `Decimal`, `Big Integer`, `Float`                        |
| Minimum Value     | Minimum value that can be set by the user.                          |
| Maximum Value     | Maximum value that can be set by the user.                          |
| Step Interval     | Specify the size of each increment (or step) of the slider control. |
| Always show value | Toggle visibility of the numeric value below the slider.            |

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Relationships
description: Leverage Directus relationships to create powerful data connections. Explore Many to One, One to Many, Many to Many, and Many to Any relationships, along with Translations for multilingual content management.
---

Directus supports all standard relationship types, as well as a few more of its own compound types, meant to streamline certain common configurations.

## Many to One (M2O)

![2 tables: One for cities each of which has a reference to a country ID, and a country table](/img/57e9702f-c8fa-48d4-90bc-a8443248d406.webp)

In a M2O relationship, multiple items from the one collection are linked to one item in a different collection. One field is added to the 'many' collection referencing the primary key of the 'one' collection.

![A cities collection item with the country many-to-one relation set to the country "Chile"](/img/3003d17c-2aa1-4664-ad58-34a755f24876.webp)

In :product-link{product="editor"}, having a M2O field does not automatically provide navigation to the related collection's items. To achieve this, the related collection requires a One to Many field to be set up.

::callout{icon="material-symbols:info-outline"}
**M2O Examples**

- Given `cities` and `countries` collections, many cities would be assigned to one country.
- Given `orders` and `customers`, many orders would be assigned to one customer.
- Given `books` and `publishers`, many books would be assigned to one publisher.

::

## One to Many (O2M)

![A one-to-many relation showing 2 tables: cities and countries](/img/574de2a6-1aa8-4594-942a-e98d79ec183e.webp)

In a O2M relationship, one item from a collection is linked to multiple items in a different collection.

![A one-to-many relation field being open](/img/63dcee0a-9ee7-4328-bd6b-bd85cc4660c9.webp)

Creating a O2M interface on the 'many' side of a M2O relationship creates an `Alias` field, which lets us access related items. This does not create a new database column as the O2M field is purely virtual. It creates an interface within the :product-link{product="editor"} to access items from an O2M perspective.

## Many to Many (M2M)

![A many-to-many relation showing 3 tables: recipes, ingredients and recipes-ingredients](/img/cb29838f-12fd-4b1a-8044-d7c7938670df.webp)


In a M2M relationship, an additional collection is created known as the junction collection. The junction collection stores the primary keys from two related collections, allowing for any number of items to be related between two collections.

You can also have a self-referencing M2M relationship that connects items in the same collection.

::callout{icon="material-symbols:info-outline"}
**Self-Referencing M2M Examples**

- Given an `articles` collection, you could configure related articles.
- Given a `users` collection, you could configure a friends list.
- Given `papers`, you could configure citations.

::

One example is "Related Articles", where each article relates to many other articles.

## Many to Any (M2A)

:video-embed{video-id="f711e94f-14c1-48dd-b00c-70a340351412"}

![A series of tables, showing blog, content_builder, headings, images and text_bodies](/img/d4cc93fb-3c0b-41fe-ae1c-5cc4f1d23979.webp)

In a M2A relationship, one collection can be related to any item in any collection. This is sometimes known as a matrix field or replicator.

When you configure a M2A in Directus, a M2A `Alias` field is created as well as a junction collection. The junction collection in a M2A relationship also stores the collection name for related collections.

::callout{icon="material-symbols:school-outline" color="secondary" to="/tutorials/getting-started/create-reusable-blocks-with-many-to-any-relationships"}
Read our tutorial on using a Builder (M2A) to create reusable page components.
::

## Translations

![An articles, languages and articles_translations table, linked by language_id](/img/f1fe2c0b-4baa-47e3-a7c1-a49c27a9852b.webp)

When you create a Translations interface in Directus, a translations O2M `Alias` field is created, as well as a `languages` collection and a junction collection between your main collection and `languages`. All translated text is stored in the junction collection.

:video-embed{video-id="2d192e76-378b-4540-9d41-2506460a50af"}

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Collection Explorer
description: Learn to filter, layout, batch edit and more with collections in the collection explorer.
---

The content module allows users to browse, filter, and search for items held in collections. When users navigate into viewing single items, they use :product-link{product="editor"}. Each page contains data from a single collection, but can display related fields for each item.

To open the collection explorer, click on the content module on the left hand side of the page.

![The content module showing a list of posts](/img/f28e21ef-07bd-4b3f-8eeb-e37aa3b388be.webp)

## Filtering Items

![The content module showing a list of posts, with the filter popup open](/img/a18b7f4a-1ea3-4d47-afa3-7671ff7be56d.webp)

You can create custom filters to display items that fulfill certain criteria.

Click on :icon{name="material-symbols:filter-list"} at the top of the page to create a filter. You can then select a field to filter by and click on the criterion to tweak what should pass for that item to be filtered.

### And / Or Groups

`AND` groups give the option to filter for items that meet all of several criteria. On the other hand, `OR` groups filter for items that meet any one of several criteria.

In order for filters to be included in `AND` or `OR` groups, filters must be indented below them in the filter UI.

### Dynamic Variables

:video-embed{video-id="cc653542-7721-4b37-8978-60fee90081dc"}

![The content module showing a list of posts, with the filters using the `$NOW` dynamic variables](/img/56b3463f-c593-4581-8dcb-a8996f9d4ad6.webp)

The following dynamic variables are built into Directus to add extra functionality to filters: 

- **`$CURRENT_USER`** — The primary key of the currently authenticated user.
- **`$CURRENT_ROLE`** — The primary key of the role for the currently authenticated user.
- **`$CURRENT_ROLES`** - An array of roles containing the `$CURRENT_ROLE` and any roles included within it.
- **`$CURRENT_POLICIES`** - An array of policies assigned to the user directly, or through their roles.
- **`$NOW`** — The current timestamp.
- **`$NOW(<adjustment>)`** - The current timestamp plus/minus a given distance, for example `$NOW(-1 year)`, `$NOW(+2 hours)`.

:video-embed{video-id="a0f37f8b-c789-4421-b6c4-e4f681028d66"}

## Layouts

Layouts are customized mechanisms for viewing and interacting with the items in a collection. You can [select a layout](/guides/content/layouts) for displaying your collection. Note that restrictions will apply depending on your collection's [data model](/guides/data-model/collections).

## Batch Editing

![Batch editing a set of posts](/img/96ae3c9e-b3b0-4c08-bdf7-d6ff8bd05c2e.webp)

By selecting more than one item in the explorer will allow you to click on :icon{name="material-symbols:filter-list"} and edit several items' fields at once to have the same value.

## Bookmarks

:video-embed{video-id="2766c70a-1fc0-46b1-ac5e-b464ca245334"}

![The list of bookmarks](/img/5279bcef-f497-413e-a568-e788c9aa5309.webp)

Bookmarks are custom views for your collections that include specified configurations, layouts, visible fields, sorting, filtering and more.

To create a bookmark, navigate to the Settings -> Bookmarks module. Here, you can create a new one by clicking on :icon{name="material-symbols:add-circle-outline-rounded"}.

You'll see the "Editing Preset" form, where you can set the name and collection, amongst layout and other values for this bookmark. Note that leaving the name field empty will make it so this bookmark is what is viewed for this collection by default.

Each Collection Page displays all Items in its Collection and comes with highly configurable Layouts for browsing, visualizing, and managing Items. The Page Header includes key action buttons for sorting, searching, filtering, creating, editing, archiving, and deleting multiple Items. To learn more, see our guide on the Collection Page.

The Content Module consists of :product-link{product="explore"}, where multiple items from a collection are displayed, and :product-link{product="editor"}, where single items can be displayed and edited.

A powerful, yet extensible, way to explore your database. Suitable for everyone in your organization with a robust permissions system.

Filters & Search: Filter your data with our powerful query builder across just one or related collections.

Layouts: Layouts are customized displays for viewing and interacting with the Items in a Collection. This makes working with specific types of data models, such as map locations or calendar events, a more human-friendly experience.

Save layout presets: Save your data layouts, filters, and sorts in presets and make them available to specific users or roles.

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Item Editor
description: Learn to create, duplicate, archive and perform other actions with items using Directus.
---

The item editor is a tailored form for managing individual items and their field values.

![Item editor](/img/e60b0053-7588-432c-830e-453fb429b10b.webp)

## Fields & Data Model

You can add fields to items by [configuring the collection's data model](/guides/data-model/fields). Here, you can also configure how the fields are displayed in the item editor.

## Creating Items

To create an item, click :icon{name="material-symbols:add-circle-outline-rounded"} in the page header to open the item page.

Fill in the fields as desired. Note that some of these will be [marked as required](/guides/data-model/fields) and need to be filled in, or be dynamic fields. Relations will be filled in here, too.

::callout{icon="material-symbols:info-outline"}
**Singletons**  

If the collection is configured as a [singleton](/guides/data-model/collections) in the data model
settings, the App will automatically open the item page when selecting the collection.

::

## Duplicating Items

![Item duplicating](/img/3ac21f31-a1e0-4506-a2cc-86a3682d4bf6.webp)

When editing an item, you can click on :icon{name="material-symbols:more-vert"} to select some advanced options, amongst them "Save as Copy". Selecting this will save a copy.

## Archiving Items

To archive an item, follow these steps, navigate to the content module and select the desired collection. Select the desired item to open the item editor. Click :icon{name="material-symbols:archive"} located in the header and a popup will appear to confirm the action.

Archived items will not show up in app, but will still be returned in API responses unless explicitly filtered out.

::callout{icon="material-symbols:info-outline"}
**Requires Configuration**  

Archiving requires an [archive field](/guides/data-model/collections) to be configured within the collection's data model
settings.

::

## Revisions

![Item revisions](/img/453e00b9-6cda-4dea-a3a8-14f5686e6564.webp)

As you update field values on items, Directus saves these revisions, and they can be compared side-by-side to the current state.

To revert an item, navigate to the content module and select the desired collection and select the desired item. Click on "Revisions" in the editor sidebar and then on the revision you wish to preview. Go to "Revisions Made" in the side menu and view the revision differences. Click :icon{name="material-symbols:settings-backup-restore"} to revert the item's values and return to the item page.

::callout{icon="material-symbols:info-outline"}
**Revision Preview**  

You will also see a "Revision Preview" button in the side menu navigation, which will let you preview all the item's
values for that revision.
::

::callout{icon="material-symbols:info-outline"}
You can also revert items [programmatically via the API](/api/revisions).
::

## Comments

![Item comments](/img/453e00b9-6cda-4dea-a3a8-14f5686e6564.webp)

You can add comments to items in the sidebar by clicking on "Comments", which will show the form for submitting one. You can use the @ button to tag specific users in your comment.

## Shares

![Item shares](/img/1ff83c92-0eb7-4cf5-a6ec-ff96801cf38c.webp)

You can create shareable links to view an item in the sidebar by clicking on Shares -> New Share.

Here, you can specify the name, password, roles allowed to access the item, as well as the start and end dates for the link's validity, followed by the maximum times a link can be used.

To share the link, click on the new share's :icon{name="material-symbols:more-horiz"} and select either "Copy Link" or "Send Link". You can also edit or destroy the share in this menu.

## Next Steps

Learn how to use [content versioning](/guides/content/content-versioning) and the [live preview](/guides/content/live-preview) functionality.

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Layouts
description: Learn to use layouts for viewing and interacting with items in a collection using Directus.
---

Layouts are customized mechanisms for viewing and interacting with the items in a collection.

## Adjust a Collection's Layout

![Layouts](/img/f801544a-adc8-4194-aee3-d15cf8bddd6a.webp)

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

![Table layout](/img/f801544a-adc8-4194-aee3-d15cf8bddd6a.webp)

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

::callout{icon="material-symbols:info-outline"}
**Manual Sorting Requires Configuration**  
Only available if you [configure a sort field](/guides/data-model/collections) in the collection's data model
settings.
::

## Card Layout

![Card layout](/img/84b95785-0e36-4630-9fbe-975264837126.webp)

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

![Calendar layout](/img/a967c260-3597-49c5-92d1-0f044ced44c5.webp)

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

::callout{icon="material-symbols:info-outline"}
**Configuration Requirements**  
To use this layout, the collection will need at least one datetime [Field](/guides/data-model/fields) to set a start time,
but ideally two datetime Fields (to set a start time and end time).
::

## Map Layout

![Calendar layout](/img/e0835568-a39e-452e-bec2-27bcc114bdd6.webp)

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

::callout{icon="material-symbols:info-outline"}
**Configuration Requirements**  
To use this Layout, the collection must have a map field configured.

<!--
@TODO configuration > data-model > fields
Link to Map Field
-->
::

## Kanban Layout

![Kanban layout](/img/0a02d810-079d-4257-83ec-d4bdd9f28d58.webp)

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

::callout{icon="material-symbols:info-outline"}
**Configuration Requirements**  
To make this layout work, you will need to configure an appropriate status [field](/guides/data-model/fields) on the
collection, then identify this field under "Group By" in the Layout Options menu.
::

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Import & Export
description: Learn to import and export multiple items stored as files using Directus.
---

The content module allows importing and exporting of multiple items from and to files respectively.

## Import Items

![Import items](/img/194f51c9-9d2d-4264-ad09-8142ff671aea.webp)

You can import JSON or CSV files into your collection as items.

To import Items from a file, navigate to the collection and click "Import / Export" in the sidebar. Click into the import search box. A file browser will open. Once a file has been selected, click on "Start Import" to import the items.

The items will now be in the collection, and the file itself will not be stored in the Directus project.

::callout{icon="material-symbols:warning-rounded" color="warning"}
The field headers in the imported file must match the field keys of the collection you're importing into. Otherwise, the column will be skipped.
::

::callout{icon="material-symbols:info-outline"}
**Importing Relational Files**
It is possible to import relational field values as well. For this task, the user performing the import will need access
permissions for the related collection.
::

::callout{icon="material-symbols:info-outline"}
**Import Error Handling**
During import operations, errors are collected up to the maximum defined by [`MAX_IMPORT_ERRORS`](/content/configuration/security-limits.md), after which the import is cancelled.
::

## Export Items

![Export items](/img/6253cd72-005d-4551-b3fd-72acd33e47f6.webp)

When exporting items, the export items menu provides granular control over exactly which items and
fields are exported, how they are exported, and where they are exported.

To export items, follow the steps below, navigate to the desired collection and select "Import / Export" from the sidebar. Click on "Export Items" and the export items menu will appear. Select the desired format from CSV, JSON, XML, or YAML and click :icon{name="material-symbols:download-for-offline"} to download the file.

## Export Items Menu

This menu provides granular control over exactly which items and fields are exported, how they are exported, and where
they are exported.

| Item | Description |
|---|---|
| **Format** | Choose to export items as CSV, JSON, XML, or YAML. |
| **Limit** | Set the maximum number of items to be exported. |
| **Export Location** | Download the export file directly to your machine or to the file library. |
| **Folder** | Choose the Folder to download to (if export location is the folder library). |
| **Sort Field** | Choose field to sort items by. |
| **Sort Direction** | Choose to sort items in ascending or descending order. |
| **Full-Text Search** | Limit exported Items to ones which matched as search results. |
| **Filter** | Limit exported items with a filter. |
| **Fields** | Add, remove, and re-order the item fields that will be exported.  |

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Live Preview
description: Learn to set up your project for live previewing items from your application.
---

Live preview allows you to show changes in your website collection before publishing and without the need to refresh the browser.

## Configure a Live Preview URL

![Data Studio configuration for Posts collection. The Preview URL is filled in with the dynamic ID.](/img/e3619c91-8917-4014-9ad1-5d7cd2b59ff4.webp)

Navigate to Settings -> Data Model and select the collection you wish to configure. In the "Preview URL" section, specify the Preview URL for your project by selecting the field you wish to use to identify your object in your application from the dropdown and entering a URL in this format:
`http://your-website-url/<field>`

### Using Live Preview with Your Application

Once configured, Directus will send a request to your application for a page with the specified URL format. For example, if you've configured the URL to be `https://mysite.com/posts/{id}`, and load the preview for the item with an `id` of `42`, then your application will receive a request to `https://mysite.com/posts/42`. You may choose to add `preview=true` to indicate to your client that it needs to treat this as a live preview. You may also choose to add an access token with the ability to view items as an additional URL query parameter.

You can then develop your application to handle that request and return a page that shows a preview of the item requested.

::callout{icon="material-symbols:warning-rounded" color="warning"}
**Using Live Preview with Static Site Generators**
If you're using a static site generator to preview your item data, be sure to develop it to render the item page on load as opposed to on build. Otherwise, it will only show the state of the item when the site itself was last built.
::

::callout{icon="material-symbols:info-outline"}
**Using Live Preview with Content Versioning**
If you've enabled [content versioning](/guides/content/content-versioning), you can pass the version identifier to your live preview URL.
::

## Previewing Item Contents in the Editor

In an item page, toggle "Enable Preview" at the top of the page. Whenever you create or edit an item in your collection
and “click” save, you should see a live preview of the item on the right-hand side of the screen.

![Live preview of a post](/img/ae834006-2b0b-40df-87aa-66e5c2da1987.webp)

Clicking on :icon{name="material-symbols:devices"} also lets you preview your content on desktop and mobile screens, while :icon{name="material-symbols:open-in-new"} allows you to pop the live preview out into a separate window.

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Content Versioning
description: This guide covers the process of enabling and utilizing Content Versioning in Directus.
---

:video-embed{video-id="0bfed0fe-2c73-4528-8a6a-d3b39b4c0528"}

Content versioning allows teams to create and manage different versions of their content. There are several reasons to
use content versioning, including drafting content without publishing it, and more ways to collaborate effectively.

## Concepts

- **Version**: a version of an item is a snapshot that gets copied from the current version or main item, allowing you to safely make changes and later promote to be the main item.
- **Main**: the main item is the default item that is displayed to users. It is the "source of truth" for all versions.
- **Promote**: when a version is promoted, it becomes the main item that is displayed to users.
- **Revision**: revisions are individual changes to items made within a version or main item. Directus keeps track of all changes made, so you're able to view the history of modifications and revert to a previous state.

::callout{icon="material-symbols:info-outline" to="/guides/content/live-preview"}
**Using Versions in Live Preview**  
The version field is a dynamic variable can be added to the live preview URL so you can preview a specific version of an item. Check out more about live previews.
::

## Setting Up Content Versioning

![Content versioning checkbox](/img/26a59b99-55e9-4185-83f3-f8945ace589e.webp)

Navigate to **Settings** > **Data Model**, select the collection that you want to enable content versioning for, and scroll down to the content versioning section. Toggle "Enable Versions" and save your data model.

## Creating a New Version

![Creating a new version in the content module](/img/comparison_create-version.png)

Open an item within your versioned collection. At the top of the item view, you will notice a dropdown with the main Content Version displayed as "main". Select "Create Version" and provide a key and a name for the new version. You can then save your new version.

::callout{icon="material-symbols:info-outline"}
**Version Source**  
All new versions originate from the main item. This implies that the main item acts as the single source of truth
for other versions.
::

## Making Changes to a Version

![Editing a version](/img/versioning_update.png)

Open the item in the newly created version, and make the desired edits to the item's content.

Upon saving the changes, you'll notice that the main item remains unaffected, while the changes are reflected only in the modified version.

## Comparing and Promoting a Version

![Promoting a version, comparing its changes](/img/comparison_promote-version.png)


Promoting a version makes it the main (current) version of your content.

### How to Promote a Version

1. Open the version you want to promote
2. Select the **"Promote Version"** option from the dropdown. 
3. In the comparison modal, review the changes:
   - Fields with differences from the main item are highlighted with color indicators
   - Review each highlighted field to understand what will change
4. Accept or reject individual changes as needed
5. Click **"Promote"** to finalize and make this version the new main item

Once promoted, this version becomes the active content, and the previous main item is preserved in the version history. 

After promoting a version, you can choose to keep or delete the version.

::callout{icon="material-symbols:info-outline"}
**Programmatically Implement Content Versioning**  
You have the option to integrate Content Versioning through the API. To learn how to accomplish this, please refer to
our [API reference documentation](/api/versions).
::

## Revisions and Content Versioning

Under the hood, revisions are stored in the `directus_revisions` collection. In bigger projects this collection
can get large.

### Managing Revision Retention

You can manage revision retention in two ways:

 - **Automatic Retention Policies**: Configure environment variables to automatically control how long revisions are kept. This allows you to balance the need for historical data with storage and performance considerations. See the [Log Retention documentation](../../configuration/logging.md#log-retention) for configuration options.
 - **Manual Cleanup**: Periodically remove some or all data from the `directus_revisions` collection. Note that manual deletion may unintentionally remove content versions that are still in use, so exercise caution when performing bulk deletions.

When implementing retention policies, consider your team's workflow and how far back you may need to revert changes before removing older revisions.

### Viewing and Restoring Revisions

You can view revisions for an item within the right sidebar of the Studio. How many fields were updated in each revision is clarified via the label e.g. **"Updated 2 Fields"**. Clicking on a revision will launch the Content Comparison Modal.

![Revisions Sidebar](/img/revisions.png)

Note that once the comparison modal is open, you can change the revision you want to compare by using the dropdown menu at the top right of the modal.

![Revisions Selector](/img/comparison_revision-selector.png)

Upon selecting a revision, you will see the differences between the current version (indicated on the left) and that revision (indicated on the right). 

![Content Comparison Modal](/img/comparison_revision-diff-indicators.png)

The 'Updated' label indicators demonstrate which fields were updated in that revision, while the change indicators assist in understanding how the selected revision currently compares to the version it is associated with. If the revision is not associated with a version, the comparison will be against the main item.

![Diff Indicators](/img/comparison_revision-diff-indicators-2.png)

When a collapsed group interface contains fields with updates or differences, a diff indicator appears next to the group name.

![Diff Indicator on Collapsed Group](/img/comparison_group-indicator.png)

When expanded, diff indicators are displayed next to each field in the group that has updates or differences.

![Diff Indicators on Expanded Group](/img/comparison_group-indicator-expanded.png)
This can be mitigated by periodically removing some or all data in this collection. Note that this could
unintentionally remove some content versions.

## API Response Structure

When working with content versioning through the API, it's important to understand how items and their versions are represented differently.

### Standard Item Response

When fetching items from a collection endpoint `/items/{collection}`, you receive the main version data:
```json
{
  "data": [
    {
      "id": 1,
      "name": "Lion King",
      "author": 1,
      "release_date": "2025-10-01T12:00:00"
    }
  ]
}
```

### Version Response

When fetching versions from the `/versions/{version}` endpoint, each version contains metadata and a delta object that shows the changes made in that version:
```json
{
    "data": {
        "id": "0e0a8110-3cab-4bfb-93d5-17662588d0d4",
        "key": "version-x",
        "name": "version-x",
        "collection": "books",
        "item": "1",
        "hash": "3284251037784c38c5bada022e579d3a484b4a09",
        "date_created": "2025-10-14T08:58:21.279Z",
        "date_updated": "2025-10-14T09:02:55.682Z",
        "user_created": "ec5f6af5-b113-4b0a-9792-67596a547fd8",
        "user_updated": "ec5f6af5-b113-4b0a-9792-67596a547fd8",
        "delta": {
            "id": "1",
            "author": 2,
            "release_date": "2025-10-05T12:00:00"
        }
    }
}
```

The `delta` object contains only the modified fields, making it easy to see exactly what changed in each version compared to the state of main at the time that version was created.

----------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------

---
title: Translations
description: Both content and the data studio can be translated into multiple languages. 
---

Localising content in Directus involves using translation strings, which are multilingual key-value pairs that you can use throughout the app. They enable you to translate things like dropdown options, placeholder text, field notes, and more.

![Translation strings](/img/d2348575-9fbb-4c38-9d9f-22e32799ded7.webp)

::callout{icon="material-symbols:info-outline"}
**Data Studio Translations**  
This article refers to translating your content in Directus. Many parts of the Data Studio are already translated into multiple languages via community contribution on Crowdin.

:video-embed{video-id="0ecac717-cbf2-4dbd-9d2b-aec232c10b0a"}
::


## Create a Translation String

:video-embed{video-id="b588e6c5-d031-4be6-aef4-e4f421f10cd5"}

![Form to create a translation string](/img/1ca1ec31-2263-4b69-b87b-95449ec98bbd.webp)

To create a translation string, navigate to Settings > Translation Strings and click on :icon{name="material-symbols:add-circle-rounded"} in the page header and a drawer will open.

Add a key and click on "Create New" to open another drawer. Select the language and type in the corresponding translation. Click on :icon{name="material-symbols:check-circle"} to confirm and add the translation.

## Use a Translation String

![Using a translation string on a field](/img/c26df052-5b97-401d-97f8-5c7c7bc29952.webp)

Throughout the settings module, you will notice certain input fields have a :icon{name="material-symbols:translate"} icon on them, meaning you have the option assign a translation string.

To assign a translation string, navigate to the input that you'd like to add a translation string to. There are two ways to assign a translation string:

- Click :icon{name="material-symbols:translate"} and a dropdown menu with all translation strings will appear.
- Type `$t:translation-string-key` and hit enter.

Choose a translation string key as desired.

::callout{icon="material-symbols:info-outline"}
**Switching Language**  
There are two ways to change the app language. Administrators can set the project's
[default language](/configuration/translations), while users can choose their own personal language preference.
::

::callout{icon="material-symbols:info-outline"}
**Adding New Translation Strings**  
You can also click ":icon{name="material-symbols:add-circle-rounded"} New Translation String" in the :icon{name="material-symbols:translate"} dropdown menu to create a new translation string on the fly.
::

## Content Translations

:video-embed{video-id="0e2eaf09-d3e4-4cca-97c8-e38e833c731f"}

With Directus, you can localize your content into several languages using a [translations field](/guides/data-model/relationships) on a given collection.

![Creating a translations field](/img/3097a653-da4f-449a-a5d5-4dcf62da73bd.webp)

Once this field is added to a collection, a few new collections will be created. One being a pre-populated `languages` collection, as well as a hidden `<colletion-name>_translations` collection.

In the "Data Model" section of the settings module, navigate into the `<colletion-name>_translations` collection and add the fields which you'd like to translate.

![Post translations collection showing it has a title and content field added](/img/ec059ce9-ece1-4353-8844-7e557a4556c4.webp)

Now, when [editing an item](/guides/content/editor) of that collection, you'll be able to add translations for those fields.

![Creating a post with a title in both Spanish and English](/img/774ac37b-1c9e-433b-80ba-deededd8e406.webp)

## RTL Support

Directus provides comprehensive Right-to-Left (RTL) language support for both content editing and the entire studio interface.

![Studio in RTL](/img/rtl-app.webp)

### Automatic RTL Detection

The studio automatically switches to RTL rendering mode when using languages that are commonly written in RTL direction, including:

- Arabic (`ar-SA`)
- Farci (`fa-IR`)
- Hebrew (`he-IL`)

### Manual RTL Override

To manually control the text direction regardless of selected language:

1. Navigate to your user profile
2. Look for the "Text Direction" option
3. Choose between:
  - "Automatic" - Follows the language's natural direction
  - "Left to Right" - Forces left-to-right direction
  - "Right to Left" - Forces right-to-left direction
