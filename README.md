# Umbraco-NodeRestrict
Sets restrictions to number of children allowed to be published under a node either by rules (parent doctype - child doctype) or by a special property in the parent node (for children of any doctype)

## Usage (using configuration file in /config folder)
If you install the package, you will find NodeRestrict.config in your /config folder with a commented example of a rule.

Example nodeRestrict.config:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nodeRestrict propertyAlias="maxChildNodes" showWarnings="false">
  <rule 
    parentDocType="Home" 
    childDocType="LandingPage" 
    maxNodes="4"
    showWarnings="true"
    customMessage=""
    customMessageCategory=""
    customWarningMessage=""
    customWarningMessageCategory=""
    >
   </rule>
</nodeRestrict>
 ```
You can define rules for parent/child sets based on document type as well as use a special property on any page that will define the number of allowed children (of any doctype) for that specific page only.

A rule has the following attributes:

* **parentDocType**: The document type alias of the parent of the document being published. 
* **childDocType**: The document type alias of the document being published.
* **maxNodes**: The maximum number of "childDocType" nodes allowed under a "ParentDocType" node.
* **showWarnings**: When set, displays warning messages regarding the number of nodes allowed if a rule is matched but the maximum number of children has not yet been reached.
* **customMessage**: Overrides the standard message when the maximum number of nodes has been reached.
* **customMessageCategory**: Overrides the standard category literal when the maximum number of nodes has been reached. Standard literal is "Publish".
* **customWarningMessage**: Overrides the standard warning message when a rule is matched but maximum number of nodes has not yet been reached.
* **customWarningMessageCategory**: verrides the standard category literal when a rule is matched but maximum number of nodes has not yet been reached. Standard literal is "Publish".

A rule is matched only when "parentDocType" and "childDocType" match. That is, the document being published must be of "childDocType" and its parent document must be of "parentDocType". In our example, the rule applies if we publish a "LandingPage" document under a "Home" document.

There are also two attributes on the root node itself:
* **propertyAlias**: The alias of the property expected to be found in a document to limit its number of children. This must be a numeric property.
* **showWarnings**: Show warning messages in case a property with the "propertyAlias" is found but the limit defined there has not been reached.


## Examples
Let's suppose that, on your site, you have pages of type "Advert" that you place under a page of type "AdvertList". You want to restrict the number of published "Advert" pages at any time to 3. You also need to display custom warning messages to let the user know that there's a limit there, as well as a custom message when the limit is reached. The rule you should create goes like this:


```xml
  <rule 
    parentDocType="AdvertList" 
    childDocType="Advert" 
    maxNodes="3"
    showWarnings="true"
    customMessage="No more adverts for you. I saved your node, but you are only allowed 3 published adverts."
    customMessageCategory="Oops! Limit Reached"
    customWarningMessage="Remember that you will not be allowed to have more than 3 adverts published here."
    customWarningMessageCategory="Warning"
    >
   </rule>
 ```
 
 Now let's suppose you have a single node where you need to limit its number of child nodes to 5. In order to do that, you must specify the "special" property alias you need to use in the config file:

```xml
<nodeRestrict propertyAlias="mySpecialPropertyAlias" showWarnings="true">
```
And have a numeric property with alias "mySpecialPropertyAlias" in your document. Then you can go and set the number 5 on it. This will behave exactly like a rule, but only for the specific node. If the node doesn't have a value for the "special" property, then this will be ignored. The "showWarnings" attribute works the same way as in rules. When applying a restriction based on the document's "special" property, it defines if warnings will be displayed. If set to false, no warnings will be displayed (only a message when the maximum number of children has been reached).
