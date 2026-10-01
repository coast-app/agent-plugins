# Components

A **component** defines a field, display element, or control in a workflow template. Components let a customer decide what a work order, asset, inspection, or other record contains and how people work with it. The customer's business name gives the field context; its configured component type determines its behavior.

This chapter covers the component model, shared configuration, how fields serve different jobs, the complete component catalogue, and system-managed fields. Start with [choosing a component](#choosing-a-component) when modeling work; use the alphabetical [catalogue](#component-catalogue) when looking up a builder name. Variants that use the same underlying type are explained together.

- [Definitions, values, and presentation](#definitions-values-and-presentation)
- [Shared configuration](#shared-configuration)
- [Choosing a component](#choosing-a-component)
- [Connected records and derived values](#connected-records-and-derived-values)
- [Subforms and embedded answers](#subforms-and-embedded-answers)
- [Component catalogue](#component-catalogue)
- [System and managed components](#system-and-managed-components)

## Definitions, values, and presentation

### A definition and its value have different owners

The workflow template owns the **component definition**: its type, identity, configuration, and applicable defaults. A workflow entity owns the **field value** recorded for that component. For example, an Asset template defines a Serial Number text field; each Asset card supplies its own serial number.

Component IDs connect definitions, values, view configuration, and other components that reference them. A field's label can change without changing that identity. A component ID must be interpreted within its owning template: copied templates can retain component IDs even though the templates themselves have new IDs.

### Some components read or control other fields

Many components collect values directly: text, dates, selected people, attachments, and so on. Others obtain their content from existing data. A Single Field Lookup reads a field through a relationship; Referenced In finds records that link to the current record; Combined Tags presents selections from other tag fields.

A Button gives access to another field's input. Info Text supplies instructions from the configuration. Neither represents a separately entered answer. Date Range coordinates two date fields. These dependencies matter when changing or copying a configuration: the visible element and the data it uses may have different component IDs.

### Views determine the experience

A view template selects and configures the fields shown in a particular experience. Create and update forms can expose different fields or require different answers while operating on the same kind of record. Cards, collections, and subforms also use view-template configuration.

A component defines the field's capabilities; the selected view configures its presentation and input behavior. [View templates](view-templates.md) explains those experiences and their layouts. Layout items such as text, images, and video are separate from component definitions and recorded answers.

## Shared configuration

### Labels, placeholders, and instructions

Customers choose the language of their workflows. A label names a field; a placeholder supplies the configured prompt or empty-state text. Controls can also have their own text, such as a timer's start and stop labels or a button's caption.

Render the configured text for the active experience. The [field label and placeholder guide](https://help.coastapp.com/hc/en-us/articles/12311428034455-Edit-a-Field-s-Label) explains these customer-facing settings. Do not derive a new sentence from the field label, pluralize it, or replace it with a more imaginative empty state. A field named “Labor” does not tell Coast whether the customer wants “Add time,” “No entries,” another language, or another phrase altogether. When no placeholder is configured, follow the control's existing fallback, which may be no placeholder. App-owned interface text and customer-authored workflow text have different owners.

### Defaults and view overrides

A default supplies an initial value where that component supports one. Different types have different default models: text can start with configured text, a Person field can select named users or the record creator, and a date can start at a configured offset. A default is not a continuously derived value or a guarantee that existing records will be rewritten when configuration changes.

View overrides can change applicable component settings for one experience. The supported settings depend on the component type; there is no universal promise that every component supports every default, placeholder, or input option. The active view's resolved configuration governs that experience.

### Visibility, editability, and requiredness

These controls answer separate questions:

| Setting | Meaning |
| --- | --- |
| Visibility | Whether the field appears in this experience. |
| Read-only | Whether the field can be edited through this experience. |
| Required | Whether the applicable form requires a value. |

Requiredness can differ between creation and update. In the web builder, a field required by the workflow template cannot be made optional by a view, and a field marked read-only by the template cannot be made editable there. A field hidden in a view may still have stored data. Display and input controls do not replace authorization: a user's permission to access or change the record is a separate decision.

### Configuration changes and dependencies

Changing a label changes how a field is described. Changing a component's referenced template, related field, options, or type-specific behavior can affect how values are interpreted, selected, or displayed. Identify those dependencies before treating an edit as cosmetic.

Archived components are retained separately from active components in the template model. Tag options also have their own archived state. These are different scopes: archiving an option does not archive its field or the records that selected it.

Archiving can preserve the meaning of historical card values and allow restoration. Deleting a field outright can leave values without enough structure to interpret them. Before changing or removing a live field, check the saved collections, forms, layouts, dashboards, automations, and integrations that depend on it. For example, a Status Tag may group a board, while a Date field may place records on a calendar or schedule a date-relative action.

## Choosing a component

Choose first by the role the information plays. These are reading paths through one component model, not extra component types. A field can span roles: Date Range presents one interval through two managed Date fields, while Related Card both stores selections and enables derived content.

| Role in the record | Start with |
| --- | --- |
| Stored inputs and observations | [Address](#address), [Date](#date), [Date Range](#date-range), [Email Address](#email-address), [File](#file), [Number](#number), [Person](#person), [Share Location](#share-location), [Signature](#signature), [Tag](#tag), [Text](#text), [Timer](#timer), [To-do List](#to-do-list), [Web URL](#web-url). |
| Connections between records | [Related Card](#related-card), [Referenced In](#referenced-in), and [Single Field Lookup](#single-field-lookup) describe the forward link, its reverse, and values read through it. See [how they fit together](#connected-records-and-derived-values). |
| Display assembled from other fields | [Combined Tags](#combined-tags) presents existing selections from Tag fields on the same record. |
| Reusable embedded answers | [Subform](#subform), [Checkbox in a subform](#checkbox-in-a-subform), [Long Text in a subform](#long-text-in-a-subform), [Single/Multi Select in a subform](#singlemulti-select-in-a-subform). See [subform scope](#subforms-and-embedded-answers). |
| Input and action controls | [Button](#button) opens another field’s input. [Scheduled Automation](#scheduled-automation) records when an automation should run relative to a date. |
| Instructions | [Info Text](#info-text) supplies configured guidance inside a subform. |
| Coast-managed fields and dependencies | [Record metadata](#record-metadata-as-fields) and [generated component relationships](#generated-component-relationships) explain fields supplied or coordinated by Coast. |

The following distinctions help translate a customer's request into those building blocks. The catalogue defines each option in detail.

| What needs to be represented? | Relevant distinction |
| --- | --- |
| A category, such as priority | A Tag selects configured options; it does not create records with their own attributes. |
| A Coast user, such as an assignee | A Person field selects users. A vendor or employee modeled as a business record instead uses a Related Card field. |
| A selected site versus a captured position | Address describes a selected place; Share Location records a device-location observation with time and user context. |
| A separately managed asset or location | A Related Card points to a record with its own identity and data. |
| A field from a selected asset | A Single Field Lookup reads related data. A separately captured value is needed when the intended meaning is a historical snapshot. |
| A short list of tasks typed into a record | A To-do List stores embedded items and their completion state. |
| A reusable procedure with different question types | A Subform selects a definition and stores the filled answers inside the parent record. |
| Text or selections within a procedure | Subform Long Text and Single/Multi Select answers can carry notes and files. Their ordinary-field counterparts store simpler text or option values. |
| A single checked/unchecked answer | A subform Checkbox stores a Boolean answer. A Tag can also have a checkbox presentation backed by two configured option values. |
| Time spent across visits or work sessions | A Timer stores time intervals. A Date or Date Range describes calendar placement. |
| Work that repeats | Date + Repeat participates in recurring record creation. Scheduled Automation schedules an action relative to a record's date. |

Operational evidence can combine photos or files, signatures, captured location, timestamps, user attribution, and subform answers. Choose the proof the process needs and place each value on the record or embedded answer where the event is captured. A service address and a captured completion location can both matter on one Work Order because they answer different questions.

## Connected records and derived values

These three components describe one connection from different positions. A Work Order's [Related Card](#related-card) field stores the selected Asset's record ID. On the Asset, [Referenced In](#referenced-in) can show Work Orders that select it; that reverse list is found through the forward link, not maintained as a second independent relationship. A [Single Field Lookup](#single-field-lookup) can read the selected Asset's serial number through the same link. The lookup reflects related data instead of storing a historical snapshot on the Work Order.

The Related Card picker can narrow eligible records with the same field-filter model used by collection views. Its criterion can also use a value from the current record, such as the Work Order's Location. The shared filter language belongs to record selection; the relationship field owns which target records and current-record context it supplies.

Put the forward link on the record where a person makes the selection. For many Work Orders referring to one Asset, each Work Order can select its Asset while the Asset shows the reverse list through Referenced In. The reverse display is useful for navigation, but an automation action that traverses Related Card selections needs a stored forward path; Referenced In does not supply one. [Record selection](record-selection.md) explains filters that can query a reverse relationship for display.

## Subforms and embedded answers

A [Subform](#subform) field selects a reusable question definition and stores the filled answers inside its parent record. The filled procedure has no separate card identity or conversation thread. Its [Checkbox](#checkbox-in-a-subform), [Long Text](#long-text-in-a-subform), and [Single/Multi Select](#singlemulti-select-in-a-subform) questions can keep notes and files alongside answers. These differ from ordinary Tag and Text values even when the builder labels look similar. [Info Text](#info-text) can supply instructions without an answer; supported subform configurations can also use separate layout instruction items.

The subform template owns the reusable question definitions, the parent record owns the filled value, and the selected subform view determines the create or update presentation. See [View templates](view-templates.md) for that presentation. Use separately modeled records and Related Card when each item needs its own identity, lifecycle, and discussion.

## Component catalogue

Entries remain alphabetical by builder-facing or common name for direct lookup. The [role table](#choosing-a-component) and the connected-record and subform explanations provide another way in; they do not change the underlying type catalogue. Type identifiers distinguish similarly named fields.

| Entry | Type | Builder names and variants |
| --- | --- | --- |
| [Address](#address) | `ADDRESS` | Address |
| [Button](#button) | `INPUT_BUTTON` | Button |
| [Checkbox in a subform](#checkbox-in-a-subform) | `AUDIT_CHECKBOX` | Checkbox |
| [Combined Tags](#combined-tags) | `COMBINED_TAGS` | Combined Tags |
| [Date](#date) | `DATE` | Date Picker, Due Date, Date + Repeat |
| [Date Range](#date-range) | `DATE_RANGE` | Date Range |
| [Email Address](#email-address) | `EMAIL` | Email Address |
| [File](#file) | `FILE` | File Upload, Image Upload |
| [Info Text](#info-text) | `STATIC_TEXT` | Info Text |
| [Long Text in a subform](#long-text-in-a-subform) | `AUDIT_TEXT` | Long Text |
| [Number](#number) | `NUMBER` | Number |
| [Person](#person) | `PERSON` | Person, Assignee |
| [Referenced In](#referenced-in) | `REFERENCED_IN` | Referenced In |
| [Related Card](#related-card) | `RELATED_CARD` | Related Card |
| [Scheduled Automation](#scheduled-automation) | `SCHEDULED_AUTOMATION` | Scheduled Automation |
| [Share Location](#share-location) | `GEOLOCATION` | Share Location |
| [Signature](#signature) | `SIGNATURE` | Signature |
| [Single Field Lookup](#single-field-lookup) | `RELATED_CARD_LOOKUP` | Single Field Lookup |
| [Single/Multi Select in a subform](#singlemulti-select-in-a-subform) | `AUDIT_TAG` | Single Select, Multi Select |
| [Subform](#subform) | `SUBFORM` | Subform; often labeled Procedure |
| [Tag](#tag) | `TAG` | Single Select, Multi Select |
| [Text](#text) | `TEXT` | Short Text, Long Text |
| [Timer](#timer) | `TIME_TRACKER` | Timer, Time Tracker |
| [To-do List](#to-do-list) | `TODO` | To-do List; often labeled Checklist |
| [Web URL](#web-url) | `URL` | Web URL |

Builder variants are configurations of a type, not additional types. The available choices and interactions depend on the containing form and client. In particular, subform questions have their own answer types, and system-managed components are not ordinary Add a Field choices.

### Address

An Address field stores a selected physical place: its displayed address information, coordinates, and place identifier. It is useful for a service address or facility location. The field's label and placeholder describe what address the customer wants collected.

An address is a place selected for the record. [Share Location](#share-location) instead captures a person's device location at a particular time. A separately managed Location with its own attributes uses a workflow template and related records.

### Button

A Button opens input for another configured field. Its definition identifies that target field and supplies the button text; changing the answer changes the target field's value. The builder's linked-field selector supports Tag fields.

For example, a “Change status” button can expose the record's Status selection. Any resulting automated effect belongs to the workflow's explicit automation configuration. The button itself does not define an arbitrary script, link destination, or new answer value.

### Checkbox in a subform

A subform Checkbox collects a checked or unchecked answer to a configured question. Its answer can also carry notes and file attachments. The question text belongs to the component definition; the answer and supporting material belong to the filled subform in its parent record.

This is the `AUDIT_CHECKBOX` answer type within a [Subform](#subform). It differs from a [Tag's checkbox presentation](#checkbox-presentation), which stores selected tag options, and a [To-do List](#to-do-list), which stores multiple items with their own text and completion state.

### Combined Tags

Combined Tags presents selections from several existing Tag fields together. Its definition names the source fields. Their options and selected values remain associated with those source fields; the combined display does not replace them with one independent choice field.

Use it when a card should show classifications such as Priority and Work Type together while keeping their configuration and values separate.

### Date

A Date field records a date/time value for the record. The customer determines its business meaning, such as a due date or inspection date. Its configuration can supply a default relative date offset and, for a recurring date, recurrence-related defaults. A [Calendar collection](view-templates.md#collection-types) uses a selected Date field to place records.

#### Date Picker and Due Date

Date Picker is the general date option. Due Date is a suggested configuration using the same type and a designated field identity. A due date is still record data; its name alone does not specify a notification or an automatic status transition.

#### Date + Repeat

Date + Repeat enables recurring record creation through the date field. It participates in a recurring schedule and uses a managed series-association component. The recurring schedule owns how occurrences are generated and extended; each occurrence is a record.

The field's recurrence configuration can provide defaults for extending the series. These concepts belong to recurring work, whereas [Scheduled Automation](#scheduled-automation) controls when an action is invoked on a record.

### Date Range

A Date Range provides one start/end experience backed by two managed Date components. Its definition identifies those start and end fields; the record's endpoint values describe the interval. The builder creates the paired date fields with the range.

Use a range to place an event or assignment on a calendar. Use a [Timer](#timer) to record elapsed work across one or more sessions. When changing a range's configuration, keep its endpoint references intact; treating it as an unrelated pair of dates loses the configured relationship.

### Email Address

An Email Address field stores email addresses as structured field values and can have configured default addresses. The record holds a list of addresses, even though the field's name is singular.

An automation can use collected contact data when configured to send email.

### File

A File field stores attachments. Each attachment includes a file location, name, content type, and applicable image dimensions. The configuration can restrict accepted content types, while labels and placeholders explain the expected material.

#### File Upload and Image Upload

File Upload is the general attachment option. Image Upload uses the same `FILE` type with an image-only content restriction. An image field is therefore a constrained attachment field, not a separate image-value type.

Attachments in a field belong to the record's structured data. Message attachments belong to messages, and images placed in a view layout supply configured content. Those surfaces can display the same kind of media while giving it different ownership. Upload limits and available capture methods belong to the applicable product plan and client contracts.

### Info Text

Info Text supplies configured instructional text inside a subform. Its text lives in the component definition; there is no per-record answer to collect.

Some subform configurations can instead use text, image, or video layout instructions. Those are layout items, not Info Text components or per-record answers. The web builder offers the instruction menu only within a subform when that capability is available, and omits Info Text from that menu in the same configuration. Recognizing both representations matters when reading an existing procedure; the visual presence of an instruction does not establish which representation owns it.

### Long Text in a subform

A subform Long Text question stores a text answer with optional notes and file attachments. The configured question and text constraints belong to the subform definition; the filled response belongs to the parent record's subform value.

Its `AUDIT_TEXT` value has an answer-plus-supporting-material structure inside a [Subform](#subform). Ordinary [Text](#text) uses the simpler text value. The shared builder label “Long Text” does not make those representations interchangeable.

### Number

A Number field records a numeric value. Its configured format determines whether people enter and see a basic number, a currency amount, or a percentage. These formats give a number different interpretation; a cost field and a percentage field must retain their format when their values are displayed or exported. See [Number Field Formats](https://help.coastapp.com/hc/en-us/articles/34613959152919-Number-Field-Formats) for the customer-facing options.

| Format | Interpretation and stored value |
| --- | --- |
| Basic | The ordinary numeric value in the customer's chosen units. |
| Currency | US-dollar amounts displayed in dollars and stored as integer cents: $12.34 is stored as 1234. |
| Percentage | Rates displayed as percentages and stored as fractions: 12.345% is stored as 0.12345. |

### Person

A Person field selects Coast users and stores their user IDs. It supports assignment or another customer-defined association with people. Defaults can identify selected users or the person creating the record.

Assignee is a suggested Person-field configuration with a designated identity. It is not another component type. Assignment is record data and must remain distinct from a person's organization membership, workspace access, and permission to change the record.

Use [Related Card](#related-card) when the selectable things are independently modeled records—such as vendor profiles—rather than Coast user accounts.

### Referenced In

Referenced In shows the reverse side of a Related Card relationship. Its definition identifies the source workflow template and the source's Related Card field. It can also specify a view for presenting the referencing records.

For example, a Work Order stores an Asset selection; the Asset's Referenced In field can show the Work Orders that selected it. The forward selection owns the link. The reverse field queries that relationship rather than storing a second independently edited list.

### Related Card

A Related Card field selects records and stores their IDs in the current record. Its configuration identifies the linked record kind and can define a selection limit, initial selections, eligible-record criteria, ordering, and per-selection quantities.

#### Defaults and prefilling

Default selections can name particular records or take related selections from another linked record. For example, the Work Order's Asset selection can supply a Location for the Work Order's own Location field. This prefills a relationship value on the work order; a Single Field Lookup instead displays data through the relationship. The configured source path identifies which field on the related record supplies the selection.

#### Selecting eligible records

The picker can use the same general field-filter model as collection views. A criterion may be fixed or use a value from the current record as an operand. For example, an Asset picker can restrict choices using a Location already selected on the work order.

This is a use of shared record-selection semantics, not a separate filtering language. The source field, target field, and current entity context are meaningful inputs to that selection.

#### Quantities and relationship kinds

When quantity is enabled, the quantity belongs to an individual selected relationship entry. “Three units of Part A used on this job” is different data from Part A's stock level. Updating that quantity does not by itself define how inventory should change.

An entity-owned number describes that entity, such as a Part's stock count. A relationship-owned quantity describes one link, such as units of that Part used by a Work Order. A parent total derived from cost lines or part usage needs a configured [calculation and write](automations.md#actions-by-effect); the relationships alone do not keep it synchronized.

A general reference links records. A field can also reference records of its own template, supporting a hierarchy when a Tree view is configured to use that relationship. The [Tree view](view-templates.md#collection-types) selects the field used as its parent relationship.

#### Forward, reverse, and lookup

The forward field owns the selection. [Referenced In](#referenced-in) finds records that selected the target; [Single Field Lookup](#single-field-lookup) reads a target field through that selection. [Connected records and derived values](#connected-records-and-derived-values) shows the three roles together.

### Scheduled Automation

A Scheduled Automation field connects an automation to a date field on the same record. Its configuration identifies the date and automation and can supply default time offsets. The record's value specifies offsets at which the configured automation should be invoked relative to that date.

The automation owns the conditions and actions performed. The field controls their date-relative scheduling. A reminder before an inspection and an action after a due date can use this relationship; the caption alone does not prescribe either effect. Recurring record creation is a separate concept.

### Share Location

Share Location records a location captured from a device, including coordinates, capture time, the associated user, and accuracy when provided. It is useful for recording where someone was when they captured the value.

An [Address](#address) names a selected place; this field records a location observation. A captured point is not a continuous location history or an independently managed Location record.

### Signature

A Signature field captures signature media as record data. Its value includes the attachment information and can include the signing timestamp and associated user. Labels and placeholders describe what the customer is asking to be signed.

### Single Field Lookup

A Single Field Lookup reads a component through a Related Card selection. The definition identifies the relationship and target field. Supported leaf fields are Address, Date, Email Address, Number, Text, and Tag. It also supports a second Related Card hop before reaching one of those leaf fields.

For example, a Work Order can read a selected Asset's serial number, or follow Asset → Model to display information from the model record. The lookup displays related data rather than establishing a historical copy. If a value must mean “asset status when reported,” capture that value explicitly at the appropriate time.

Several linked cards can contribute separate values: two selected Assets can provide two serial numbers. A compact presentation may summarize that set, while a fuller presentation exposes its individual values. The lookup does not turn those values into one independently stored answer or an automatic numeric aggregate. Empty relationships and unavailable targets can leave it with no value to display; precise ordering, overflow, and export representation belong to the corresponding surface's contract.

The builder calls this Single Field Lookup; the underlying type is `RELATED_CARD_LOOKUP`.

### Single/Multi Select in a subform

These question fields collect one or more configured answer options inside a subform. The answer can also carry notes and file attachments. The `AUDIT_TAG` definition provides the options, their labels and colors, selection mode, and applicable defaults.

Single Select and Multi Select are variants of this type. Their answer-plus-supporting-material value differs from an ordinary [Tag](#tag) field's list of selected option values. Use the containing [Subform](#subform) and actual component type to interpret an answer; the visible field label is insufficient.

### Subform

A Subform field holds structured answers inside its parent record. Its definition lists the allowed subform templates and can choose a default. The selected subform template supplies the question definitions. A parent template supports one Subform component; a subform cannot contain another Subform. Reuse or relate standalone records when the work needs a larger hierarchy.

#### Definition, selection, and filled value

The parent record's value identifies the selected subform template and contains its nested field data, with a view-template reference where applicable. The selectable definitions and the filled answers are different populations. A customer may have many possible procedures while any one record contains only its selected procedure's answers.

A filled subform is embedded data. It does not have a separate workflow-entity identity or its own conversation thread. When a process needs independently discoverable records with their own lifecycle and discussion, model those as records and link them explicitly.

#### Questions and presentation

Subform-specific question types include [Single/Multi Select](#singlemulti-select-in-a-subform), [Long Text](#long-text-in-a-subform), and [Checkbox](#checkbox-in-a-subform); these can retain notes and attachments with an answer. The web subform builder also offers these ordinary fields: [Person](#person), [Date](#date), [Email Address](#email-address), [Web URL](#web-url), [Number](#number), [File or Image Upload](#file), [Address](#address), [Share Location](#share-location), [Signature](#signature), [To-do List](#to-do-list), and [Timer](#timer). This is its subform field menu, not permission to place every ordinary component inside a procedure: Related Card, Date Range, and nested Subform, for example, are absent. [Info Text](#info-text) supplies configured guidance where offered; some subform configurations instead offer layout instruction items.

Subform view templates control the presented fields and their overrides, including create and update experiences. A procedure's definition, filled values, and selected presentation remain distinct, just as they do for other configured forms.

### Tag

A Tag field selects from configured options. Each option has a stable value plus presentation such as its label and color; the record stores the selected option values. The builder offers Single Select and Multi Select variants of the same `TAG` type.

#### Options, defaults, and editing

Configuration governs the selection mode, available options, default selection, and whether people may add or edit options from the working experience. Tag options can be reordered, renamed, or archived. A selected label should be resolved through its option value, rather than used as the identifier.

Recurring work can have different tag defaults for its initial and subsequent records. The recurring schedule determines those records; the tag definition supplies the configured values.

#### Checkbox presentation

A single-select Tag with exactly two active options can use a checkbox presentation that maps checked and unchecked states to those option values. The answer remains tag data. This can express customer-defined pairs such as Complete/Incomplete without turning the field into an `AUDIT_CHECKBOX` answer.

A [List collection](view-templates.md#collection-types) can select a Tag field for its checklist presentation.

Tag fields describe categories. A choice that needs its own attributes, discussion, or lifecycle should be considered as a separately modeled record with a Related Card relationship.

### Text

A Text field stores a text value and can supply default text. Its definition controls whether input is single-line or multiline and the applicable maximum length.

Short Text and Long Text are builder configurations of `TEXT`. The distinction changes input and configured constraints, not the basic value type. A text field can also be chosen as the record's displayed title; that use does not turn its value into a unique record identifier.

The Long Text question inside a subform uses [a different answer type](#long-text-in-a-subform), which supports notes and attachments alongside the answer.

#### The record title

The title field is a Text component referenced by the workflow template. It is not an additional generated component. The template owns the selection, and the record supplies its value. See [workflow templates](templates.md) and [records](records.md) for the surrounding model.

### Timer

A Timer records time intervals on a record. Each interval has a start and an optional end. A running interval has no end; completed intervals contribute durations, and several intervals can accumulate time across separate work sessions.

#### Recording and maintaining time

The timer supports starting and stopping as work happens and managing entries manually, including adding, editing, and deleting entries. Configuration supplies the start and stop button text and can enable automatic start. Read-only behavior and selected-view configuration still apply.

One field can represent one continuous period or work accumulated over repeated starts and stops. Manual management lets the recorded time be corrected independently of the immediate start/stop interaction. Client support for a particular interaction must be established in that client's implementation.

For example, one Work Order's Labor timer can total several visits. If each visit needs its own assignee, approval, rate, discussion, or reporting lifecycle, model separate related time-entry records.

#### Meaning comes from the workflow

The same time model can support labor, shifts, clock-in/out, equipment downtime, or billable work. Coast does not derive billing, shift rules, or downtime transitions from a field's name. Those are customer configurations using this general time-recording capability.

### To-do List

A To-do List stores a list of items within one record. Each item has its own text and completion state, with an item identifier for editing. People can add or remove items and mark them complete or incomplete.

These items are embedded field data, not separate cards. Use a To-do List for steps written directly into the record. Use a [Subform](#subform) when a reusable procedure needs configured questions of different types and supporting answers.

### Web URL

A Web URL field stores web links as structured URL values. The record can hold multiple entries. The field's label and placeholder tell the person what kinds of links are expected, such as manuals or a vendor's documentation.

Use a File field when the content itself should be attached to the record.

## System and managed components

Some fields are supplied or coordinated by Coast rather than added as independent questions. Their visible presence must not be used to infer that people can edit them or that each has an independent value.

### Record metadata as fields

Coast supplies managed component definitions for record metadata. These let the configured experience present metadata through familiar field types. The set carried by a particular template and the active view determine what appears.

| Default field name | Type | Meaning |
| --- | --- | --- |
| Created By | Person | The user who created the record. |
| Created On | Date | The record's creation timestamp. |
| Updated On | Date | The record's update timestamp. |
| Card ID / Card ID Number | Number | A generated sequence number used to refer to the card; distinct from its UUID and displayed title. |
| Permalink | Web URL | The generated link to the record, when available. |
| Deleted | Date | Deletion metadata managed by the record lifecycle. |

These are server-managed fields. They are not six new selectable types, and their names do not establish that every template displays them. Configuring their presentation is different from editing their values.

### Generated component relationships

| Component or capability | Dependent configuration |
| --- | --- |
| Date Range | Two Date components supply its start and end values. |
| Date + Repeat | A server-managed Entity Batch component associates a record with its recurring series. |

The model distinguishes a component managed by another component, a locked configuration, and a server-managed value. These describe different constraints. None is a substitute for the user's access permissions.
