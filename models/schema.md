# Schema file

This is the main file that defines a model. It represents the schema, i.e the list of fields (and its types) that the model will have.


## Mongoose types
Route Injector supports the following Mongoose types:

#### String
```javascript
name: {type: String},
otherWay: String
```

#### Number
```javascript
number: {type: Number},
otherWay: Number
```

#### Date
```javascript
date: {type: Date},
otherWay: Date
```

#### Array
```javascript
myArray:[String],
otherWay: [{type: String}]
```

#### Mixed
```javascript
mixedType: {type: mongoose.Schema.Types.Mixed}
```

As well as embedded types, for example:

```javascript
var schema = new Schema({
    name:
    {
        displayName: {type: String, class: 'col-md-4'},
        firstName:{type: String, class: 'col-md-4'},
        lastName: {type: String, class: 'col-md-4'}
    },
    location:
    {
        address: { type: String, class: 'col-md-12'},
        zip: { type: String, class: 'col-md-3'},
        town:{ type: String, class: 'col-md-3'},
        state: { type: String, class: 'col-md-3'},
        country: { type: String, class: 'col-md-3'}
    }
});
```
You can also define specific subschemas that can be reusable.

```javascript
var stepSchema = new Schema({
        description: {type: String, format: 'textarea', rows: 5},
        cookTime: {type: Number, title: "Cook Time (in seconds)"}
    },
    { id: false, _id: false }
);

var groupSchema = new Schema({
        name: {type: String},
        steps: [stepSchema]
    }, 
    { id: false, _id: false }
);

var schema = new Schema({
        title: {type: String},
        groups: [groupSchema]
}
```

## Other Types

Route Injector also provides a few additional types not included in Mongoose.

#### RImage
```javascript
image: injector.types.RImage
```
## Attributes

The behaviour of this types can be modified by using additional attributes. For adding attributes you should use the extended type field declaration as shown in the next example:

```javascript
text: {type:String, <attribute>}
```

The available attributes are:

| Attribute                         | Description                                                                   | Applies to types                   |
|-----------------------------------|-------------------------------------------------------------------------------|------------------------------------|
| [title](#title)                   | Title for the field (if not present uses a humanized version of field name)   | All                                |
| [description](#description)       | Additional description for the field                                          | All                                |
| [required](#required)             | If the field is mandatory or not                                              | All                                |
| [readonly](#readonly)             | If the field is read only or not                                              | All                                |
| [unique](#unique)                 | If the field value must be unique in the collection or not                    | All                                |
| [default](#default)               | Default value of the field                                                    | All                                |
| [format](#format)                 | Specifies a format for the type, for example HTML or Textarea                 | String, Number                     |
| [class](#class)                   | CSS style for the container of the field                                      | All                                |
| [fieldClass](#fieldClass)         | CSS style fot the field                                                       | All                                |
| [minValue](#minValue)             | Minimal value of the field                                                    | Number                             |
| [maxValue](#maxValue)             | Maximum value of the field                                                    | Number                             |
| [min](#min)                       | Minimal value of the field                                                    | Number                             |
| [max](#max)                       | Maximum value of the field                                                    | Number                             |
| [enum](#enum)                     | Allowed values                                                                | String, Number                     |
| [map](#map)                       | Allowed values and their representable names                                  | String, Number                     |
| [rows](#rows)                     | Number of rows in the textarea                                                | String (Textarea)                  |
| [limitToOptions](#limitToOptions) | If the values in the enum are recommended or mandatory                        | String, Number (Enum)              |
| [ref](./references.md)            | Specifies which other model this field references                             | All                                |
| [denormalize](./denormalize.md)   | Specifies how to denormalize the field (copy values from the referenced one)  | Mixed                              | 
| [dependsOn](./dependencies.md)    | Specifies how to udate the field when a related field is modified             | All                                |

TODO What's the correct max or maxValue ???

###<a name="class"></a>class
This attribute specifies an additional CSS style for the the container of the field.

```javascript
rank: {type: String, class: "hidden", readonly: true}
```
This example adds a rank field of type string that will not be shown, and as a safety measure is also readonly.

```javascript
phone: {type: Number, class: 'col-md-6'}
```
The second example is a field of type number that uses the Bootstrap class col-md-6 for nice formatting of the backoffice.

###<a name="fieldClass"></a>fieldClass

This attribute specifies an additional CSS style for the the field.

TODO: Example

###<a name="format"></a>format

The format attributes allow to specify specific visualizations for general types like string or number. The allowed values (without any additional plugin) are: [html](#html), [textarea](#textarea), [rating](#rating), [time-seconds](#time-seconds) and [button](#button).

```javascript
name: {type: String, format: <Attribute>}
```

####<a name="html"></a>html
This format allows to render a string field as an small HTML editor on the backoffice.

```javascript
name: {type: String, format: 'html'}
```

####<a name="textarea"></a>textarea
```javascript
name: {type: String, format: 'textarea', rows: 5}
```

####<a name="rating"></a>rating
```javascript
name: {type: Number, format: 'rating', minValue:1, maxValue: 3}
```

####<a name="time-seconds"></a>time-seconds
```javascript
name: {type: Number, format: 'time-seconds'}
```

####<a name="button"></a>button
```javascript
action1: {type: String, format: 'button', action:'api', method:'GET', url:'/my/url', title: 'Call api function'}
action2: {type: String, format: 'button', action:'function',  func: 'insideFunction'}
```

### Modifiers
#### readonly
#### unique