## dualListBoxFSC
This screen component accepts three kinds of input: comma-separated values, a collection of Strings, and an Apex-defined type called FieldDescriptor that makes it easy to work with Salesforce objects and field names. 

 ### Attributes
Admin must specify only one of following  allOptionsStringCollection;allOptionsCSV;options. This attributes specify all options for dualListBoxFSC. Output attributes, however, can be used all together, so in case if in flows we need to get output in all possible types (csv;list,object) we can refer to corresponding output attributes (selectedOptionsCSV;selectedOptionsStringList;values)
#### allOptionsStringCollection
Array of strings (['value1','value2','value3','value4']) representing the full set of available options.
#### selectedOptionsStringList
Output array of strings (['value1','value4']) Displays on the right side of the dual listbox
#### allOptionsCSV
Comma-separated string of values ('Id,Name,WebSite') 
#### selectedOptionsCSV 
comma-separated string of values. ('Id,WebSite') Displays on the right side of the dual listbox 
#### allOptionsFieldDescriptorList
The full set of available options.This attribute expects a data structure of apex://FieldDescriptor[] . This data structure is generated by the GetFieldInformation Flow Action, which is packaged with this component. For more information on apex-defined data types, see (https://unofficialsf.com/the-salesforce-automation-and-decisioning-wiki/apex-data/)
#### selectedOptionsFieldDescriptorList
The selected set of available options.This attribute expects a data structure of apex://FieldDescriptor[] . This data structure is generated by the GetFieldInformation Flow Action, which is packaged with this component. For more information on apex-defined data types, see (https://unofficialsf.com/the-salesforce-automation-and-decisioning-wiki/apex-data/)
## GetFieldInformation Invocable Action
This action takes the name of an object and returns a List of field description information. The return type is an Apex-defined type called FieldDescriptor that currently includes Name, Label, Type, and Required. This data structure can be passed to dualListBoxFSC and other components or actions that support this Apex-defined type (see below).
### Attributes
#### Input
##### objectName (type String)
Any standard or custom object in Salesforce (examples: Account, Case, Custom__c)
#### Output
##### fields (type List<FieldDescriptor>)
Returns List of field descriptions for the object. 
### About the FieldDescriptor Apex-defined Type
Apex-defined types allows Apex classes to be manipulated via invocable actions and Flow. 
Currently FieldDescriptor supports the following attributes: Name, Label, Type, Required. 
The definition of this type is included with this action in the file FieldDescriptor.cls
For more information on apex-defined data types, see (https://unofficialsf.com/the-salesforce-automation-and-decisioning-wiki/apex-data/)
 
 -----------------------
# EnhancedDualListBox
The core EnhancedDualListBox component is based on standard lightning-dual-listbox lwc component and inherits all inputs from it. It adds the ability to use List<String> or arbitrary JSON objects. When objects are used you can specify which key/value pair should be displayed and which should used for the underlying value.
### Attributes
#### allOptions
The set of total available choices, which will be renderred in left side of dualListBox unless you also pass in a selectedOptions. This is a string value. Three string formats are supported:
csv - comma separated string of values ('Id,Name,WebSite')
list - array of strings (['value1','value2','value3','value4'])
object - any JSON object with own set of fields ({"fieldName": "myCustomField", "fieldValue":"foobar"})
#### allOptionsStringFormat
Specifies the format used in the 'allValues' input attribute. Allowable values are 'csv','list', 'object'.
#### selectedOptions
Represents currently selected values, displayed on the right side of the component. All members of this attribute should also be members of the allValues attribute. Two string formats are supported:
csv - comma separated string of values ('Id,Name,WebSite')
list - array of strings (['value1','value2','value3','value4'])
#### selectedValuesStringFormat
Specifies the format used in the 'selectedOptions' attribute.Supported values are 'csv' and 'list' and 'object'.
### Attributes Used Only When selectedValuesStringFormat is 'Object'
#### useWhichObjectKeyForLabel(default='label')
  Same as above but for label.Used only when selectedValuesStringFormat = 'object'.
#### useWhichObjectKeyForData (default='value')
 Specifies which object key should be accessed for the value. In case if object that is passed on input does not contain field 'value', or developer wants to use some other field for value. Used only when selectedValuesStringFormat = 'object'.
#### useObjectValueAsOutput(default=false)
If set to true, output parameters will contain values, otherwise labels.Used only when selectedValuesStringFormat = 'object'.
A primary use case for objects is the ability to pass both Labels and ApiNames for some set of data into the dual list box, have the Labels show up to the user, but have the ApiNames actually get used under the surface. This works around the fact that the dual list box base component is only aware of strings. DualListBox will display strings but then associate them back to their underlying objects.
For example, suppose you're creating an approval process application that works with any object type. You want to let a user pick which fields from their object will get displayed when approvers are deciding whether or not to approve the object (In the existing Salesforce Approval Processes application, it looks like this: https://drive.google.com/file/d/16wWXoJLgdld0K9pxdoV-XVqvkIi3ddOI/view?usp=sharing).
Your data structure might look like this:
{[{"FieldName": "Ships Name",
 "Value":"Name"},
 {"FieldName": "Requested Enhancement",
 "Value":"Requested_Enhancement__c"}]}
 You want to display the FieldNames but return the Values.
 In this case, you would set:
 allValuesStringFormat = 'object'
 selectedValuesStringFormat = 'object'
 useWhichObjectKeyForLabel = 'FieldName'
 useWhichObjectKeyForData = 'Value'
 useObjectValueAsOutput = 'true' (edited)