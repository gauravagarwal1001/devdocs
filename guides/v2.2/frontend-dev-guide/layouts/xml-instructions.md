---
group: frontend-developer-guide
title: Layout instructions
functional_areas:
  - Frontend
---

## What's in this topic {#fedg_layout_xml-instruc_overview}

There are two possible ways to customize page layout in Magento:

* Changing {% glossarytooltip 73ab5daa-5857-4039-97df-11269b626134 %}layout{% endglossarytooltip %} files.
* Altering templates.

To change the page wireframe, modify the [page layout] files; all other customizations are performed in the [page configuration] or [generic layout] files. 

Use these {% glossarytooltip bcbc9bf8-3251-4b3c-a802-07417770af3b %}layout instructions{% endglossarytooltip %} to:

*  Move a page element to another parent element.
*  Add content.
*  Remove a page element.

The basic set of instructions is the same for all types of layout files. This topic describes these basic instructions. For details about how they are used in a particular layout file type, please refer to the [Layout file types] topic.

## Common layout instructions {#fedg_layout_xml-instruc_ex}

Use the following layout instructions to customize your layout:

* [`<block>`](#fedg_layout_xml-instruc_ex_block) 
* [`<container>`](#fedg_layout_xml-instruc_ex_cont) 
* [`before` and `after` attributes](#fedg_xml-instrux_before-after)
* [`<action>`](#fedg_layout_xml-instruc_ex_act)
* [`<referenceBlock>` and `<referenceContainer>`](#fedg_layout_xml-instruc_ex_ref)
* [`<move>`](#fedg_layout_xml-instruc_ex_mv)
* [`<remove>`](#fedg_layout_xml-instruc_ex_rmv)
* [`<update>`](#fedg_layout_xml-instruc_ex_upd)
* [`<argument>`](#argument)

### block {#fedg_layout_xml-instruc_ex_block}

Defines a block.

**Details:** A block is a unit of page output that renders some distinctive content (anything visually tangible for the end-user), such as a piece of information or a user interface element.

Blocks employ templates to generate HTML. Examples of blocks include a {% glossarytooltip 50e49338-1e6c-4473-8527-9e401d67ea2b %}category{% endglossarytooltip %} list, a mini cart, product tags, and product listing.

{:.bs-callout .bs-callout-info}
The `class` attribute is no longer required in versions `2.2.1` and above as it will default to `Magento\Framework\View\Element\Template`. **In versions lower than `2.2.1`, the `class` attribute is still required.**

{:.bs-callout .bs-callout-info}
We recommend always adding a `name` to blocks. Otherwise, it is given a random name.

| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `class` | Name of a class that implements rendering of a particular block. An object of this class is responsible for actual rendering of block output. | class name | no |
|`name` | Name that can be used to address the block to which this attribute is assigned. The name must be unique per generated page. If not specified, an automatic name will be assigned in the format <code>ANONYMOUS_<em>n</em></code> | 0-9, A-Z, a-z, underscore (_), period (.), dash (-). Should start with a letter. Case-sensitive. | no |
| `before` | Used to position the block before an element under the same parent. The element name or alias name is specified in the value. Use dash (-) to position the block before all other elements of its level of nesting. See [before and after attributes](#fedg_xml-instrux_before-after) for details. | Possible values: element name or dash (-) | no |
| `after` | Used to position the block after an element under the same parent. The element name or alias name is specified in the value. Use dash (-) to position the block after all other elements of its level of nesting. See [before and after attributes](#fedg_xml-instrux_before-after) for details. | Possible values: element name or dash (-) | no |
| `template` | A template that represents the functionality of the block to which this attribute is assigned. | template file name | no |
| `as` | An alias name that serves as identifier in the scope of the parent element. | 0-9, A-Z, a-z, underscore (_), period (.), dash (-). Case-sensitive. | no | 
| `cacheable` | Defines whether a block element is cacheable. This can be used for development purposes and to make needed elements of the page dynamic. | `true` or `false` | no |


To pass parameters use the [`<argument></argument>`](#argument) instruction. 

### container {#fedg_layout_xml-instruc_ex_cont}

A structure without content that holds other layout elements such as blocks and containers.

**Details:** 
A container renders child elements during view output generation. It can be empty or it can contain an arbitrary set of `<container>` and `<block>` elements.

{:.bs-callout .bs-callout-info}
We recommend always adding a `name` to containers. Otherwise, it is given a random name.

| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `name` | A name that can be used to address the container in which this attribute is assigned. The name must be unique per generated page. | A-Z, a-z, 0-9, underscore (_), period (.), dash (-). Should start with a letter. Case-sensitive. | no |
| `label` | An arbitrary name to display in the web browser. | any| no |
| `before` | Used to position the container before an element under the same parent. The element name or alias name is specified in the value. Use dash (-) to position the block before all other elements of its level of nesting. See [before and after attributes](#fedg_xml-instrux_before-after) for details. | Possible values: element name or dash (-). | no |
| `after` | Used to position the container after an element under the same parent. The element name or alias name is specified in the value. Use dash (-) to position the block after all other elements of its level of nesting. See [before and after attributes](#fedg_xml-instrux_before-after) for details. | Possible values: element name or dash (-). | no |
| `as` | An alias name that serves as identifier in the scope of the parent element. | 0-9, A-Z, a-z, underscore (_), period (.), dash (-). Case-sensitive. | no |
| `output` | Defines whether to output the root element. If specified, the element will be added to output list. (If not specified, the parent element is responsible for rendering its children.) | Any value except the obsolete `toHtml`. Recommended value is `1`. | no |
| `htmlTag` | Output parameter. If specified, the output is wrapped into specified HTML tag. | Any valid HTML 5 tag. | no |
| htmlId | Output parameter. If specified, the value is added to the wrapper element. If there is no wrapper element, this attribute has no effect. | Any valid HTML 5 `id` value. | no |
| `htmlClass` | Output parameter. If specified, the value is added to the wrapper element. If there is no wrapper element, this attribute has no effect. | Any valid HTML 5 `class` value. | no |


Sample of usage in layout:

```xml
<container name="div.sidebar.additional" htmlTag="div" htmlClass="sidebar sidebar-additional" after="div.sidebar.main">
    <container name="sidebar.additional" as="sidebar_additional" label="Sidebar Additional"/>
</container>
```

This would add a new column to the page layout.

### before and after attributes {#fedg_xml-instrux_before-after}

To help you to position elements in a specific order suitable for design, SEO, usability, or other requirements, Magento software provides the `before` and `after` layout attributes.
These optional attributes can be used in layout XML files to control the order of elements in their common parent.

The following tables give a detailed description of the results you can get using the `before` and `after` attributes. The first table uses a block a as positioned element.

| Attribute | Value | Description |
|:------- |:------ |:------ |
| `before` | Dash (-) | The block displays before all other elements in its parent node. |
| `before` | [element name] | The block displays before the named element. |
| `before` | empty value or [element name] is absent | Use the value of `after`. If that value is empty or absent as well, the element is considered as non-positioned. |
| `after` | Dash (-) | The block displays after all other elements in its parent node. |
| `after` | [element name] | The block displays after the named element. |
| `after` | empty value or [element name] is absent | Use the value of `before`. If that value is empty or absent as well, the block is considered as non-positioned. |


#### Examples {#examples}

| Situation | Result |
|:------- |:------ |
| Both `before` and `after` attributes are present | `after` takes precedence. |
| Both `before` and `after` attributes are absent or empty | The element is considered as non-positioned. All other elements are positioned at their specified locations. The missing element displays at a random position that doesn't violate requirements for the positioned elements. |
| Several elements have `before` or `after` set to dash (-) | All elements display at the top (or bottom, in case of the after attribute), but the ordering of group of these elements is undefined. |
| The `before` or `after` attribute's value refers to an element that is not located in the parent node of the element being defined. | The element displays at a random location that doesn't violate requirements for the correctly positioned elements. |


### action {#fedg_layout_xml-instruc_ex_act}

{:.bs-callout .bs-callout-warning}
The `<action>` instruction is deprecated. If the method implementation allows, use the [`<argument>`](#argument) for [`<block>`](#fedg_layout_xml-instruc_ex_block) or [`<referenceBlock>`](#fedg_layout_xml-instruc_ex_ref) to access the block public API.

Calls public methods on the block API.

**Details:** Used to set up the execution of a certain method of the block during block generation; the `<action>` node must be located in the scope of the `<block>` node.

```xml
<block class="Magento\Module\Block\Class" name="block">
    <action method="setText">
        <argument name="text" translate="true" xsi:type="string">Text</argument>
    </action>
    <action method="setEnabled">
        <argument name="enabled" xsi:type="boolean">true</argument>
    </action>
</block>
```

`<action>` child nodes are translated into block method arguments. Child nodes names are arbitrary. If there are two or more nodes with the same name under `<action>`, they are passed as one array.

| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `method` | Name of the public method of the block class this tag is located in that is called during block generation. | block method name | yes |


To pass parameters, use the [`<argument></argument>`](#argument) instruction.

### referenceBlock and referenceContainer {#fedg_layout_xml-instruc_ex_ref}
Updates in `<referenceBlock>` and `<referenceContainer>` are applied to the corresponding `<block>` or `<container>`.

For example, if you make a reference by `<referenceBlock name="right">`, you're targeting the block `<block name="right">`.

To pass parameters to a block use the [`<argument></argument>`](#argument) instruction.

| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `remove` | Allows to remove or cancel the removal of the element. When a container is removed, its child elements are removed as well. | true/false | no |
| `display` | Allows you to disable rendering of specific block or container with all its children (both set directly and by reference). The block's/container's and its children' respective PHP objects are still generated and available for manipulation. | true/false | no |


- The `remove` attribute is optional and its default value is `false`.

    This implementation allows you to remove a block or container in your layout by setting the remove attribute value to `true`, or to cancel the removal of a block or container by setting the value to `false`.
     
    ```xml
    <referenceBlock name="block.name" remove="true" />
    ```

- The `display` attribute is optional and its default value is true.- 

    You are always able to overwrite this value in your layout.
    In situation when remove value is true, the display attribute is ignored.
     
    ```xml
    <referenceContainer name="container.name" display="false" />
    ```

### move {#fedg_layout_xml-instruc_ex_mv}

Sets the declared block or container element as a child of another element in the specified order.

```xml
<move element="name.of.an.element" destination="name.of.destination.element" as="new_alias" after="name.of.element.after" before="name.of.element.before"/>
```

- `<move>` is skipped if the element to be moved is not defined.
- If the `as` attribute is not defined, the current value of the element alias is used. If that is not possible, the value of the `name` attribute is used instead.
- During layout generation, the `<move>` instruction is processed before the removal (set using the `remove` attribute). This means if any elements are moved to the element scheduled for removal, they will be removed as well.

| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `element` | Name of the element to move. | element name | yes |
| `destination` | Name of the target parent element. | element name | yes |
| `as` | Alias name for the element in the new location. | 0-9, A-Z, a-z, underscore (_), period (.), dash (-). Case-sensitive. | no | 
| `after` or `before` | Specifies the element's position relative to siblings. Use dash (-) to position the block before or after all other siblings of its level of nesting. If the attribute is omitted, the element is placed after all siblings. | element name | no |


### remove {#fedg_layout_xml-instruc_ex_rmv}

`<remove>` is used only to remove the static resources linked in a page `<head>` section.
For removing blocks or containers, use the `remove` attribute for [`<referenceBlock>` and `<referenceContainer>`](#fedg_layout_xml-instruc_ex_ref).

```xml
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
   <head>
      <!-- Remove local resources -->
      <remove src="css/styles-m.css" />
      <remove src="my-js.js"/>
      <remove src="Magento_Catalog::js/compare.js" />

      <!-- Remove external resources -->
      <remove src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css"/>
      <remove src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js"/>
      <remove src="http://fonts.googleapis.com/css?family=Montserrat" /> 
   </head>
</page>
```

### update {#fedg_layout_xml-instruc_ex_upd}

Includes a certain layout file.

```xml
<update handle="{name_of_handle_to_include}"/>
```

The specified [handle] is "included" and executed recursively.

### argument {#argument}

Used to pass an argument. Must be always enclosed in [`<arguments>`](#arguments).
 
| Attribute | Description | Values | Required? |
|:------- |:------ |:------ |:------ |
| `name` | Argument name. | unique | yes |
| `xsi:type` | Argument type. | `string|boolean|object|number|null|array|options|url|helper` | yes |
| `translate` | | `true|false` | no |

To pass multiple arguments use the following construction:

```xml
<arguments>
   <argument name="item1" xsi:type="string">Custom string</argument>
   <argument name="item2" xsi:type="boolean">true</argument>
   ...
</arguments>
```

Arguments values set in a layout file can be accessed in [templates] using the `getData('{ArgumentName}')` and `hasData('{ArgumentName}')` methods. The latter returns a boolean defining whether there's any value set. 
`{ArgumentName}` is obtained from the `name` attribute the following way: for getting the value of `<argument name="some_string">` the method name is `getData('some_string')`.

**Example**:

Setting a value of `css_class` in the `[app/code/Magento/Theme/view/frontend/layout/default.xml]` layout file:

```xml
<arguments>
    <argument name="css_class" xsi:type="string">header links</argument>
</arguments>
```

Using the value of `css_class` in `[app/code/Magento/Theme/view/frontend/templates/html/title.phtml]`:

```php
$cssClass = $this->hasCssClass() ? ' ' . $this->getCssClass() : '';
```

#### Argument types examples

As was described above the argument attribute can be added with different types.
There are examples of all argument types.

- The *string* type:

```xml
<argument name="some_string" xsi:type="string" >Some String</argument>
```

- The *boolean* type:

```xml
<argument name="is_active" xsi:type="boolean" >true</argument>
```

- The *object* type:

```xml
<argument name="viewModel" xsi:type="object" >Vendor\CustomModule\ViewModel\Class</argument>
```

The `Vendor\CustomModule\ViewModel\Class` class should implement the `\Magento\Framework\View\Element\Block\ArgumentInterface` interface.

- The *number* type:

```xml
<argument name="some_number" xsi:type="number" >100</argument>
```

- The *null* type:

```xml
<argument name="null_value" xsi:type="null" />
```

- The *array* type:

```xml
<argument name="custom_array" xsi:type="array">
   <item name="array_key_one" xsi:type="string">First Item</item>
   <item name="array_key_two" xsi:type="string">Second Item</item>
   ...
</argument>
```

- The *options* type:

```xml
<argument name="options" xsi:type="options" >Vendor\CustomModule\Source\Options\Class</argument>
```

The `Vendor\CustomModule\Source\Options\Class` class should implement the `\Magento\Framework\Data\OptionSourceInterface` interface.

- The *url* type:

```xml
<argument name="shopping_cart_url" xsi:type="url" path="checkout/cart/index" />
```

- The *helper* type:

```xml
<argument name="helper_method_result" xsi:type="helper" helper="Vendor\CustomModule\Helper\Class::someMethod" >
  <param name="paramName">paramName</param>
    ...
</argument>
```

The *helper* can use only public methods. In this example the `someMethod()` method should be public.
The argument with *helper* type can contain `param` items which can be passed as a helper method parameters.

#### Obtain arguments examples in template

These argument examples can be taken in the template like in the following example:

```php
<?php
/** @var \Magento\Framework\View\Element\Template $block */

/** @var string $someString */
$someString = $block->getData('some_string');
/** @var bool $isActive */
$isActive = $block->getData('is_active');
/** @var Vendor\CustomModule\ViewModel\Class|\Magento\Framework\View\Element\Block\ArgumentInterface $viewModel */
$viewModel = $block->getData('viewModel');
/** @var string|int|float $someNumber */
$someNumber = $block->getData('some_number');
/** @var null $nullValue */
$nullValue = $block->getData('null_value');
/** @var array $customArray */
$customArray = $block->getData('custom_array');
/** @var array $options */
$options = $block->getData('options');
/** @var string $shoppingCartUrl */
$shoppingCartUrl = $block->getData('shopping_cart_url');
/** @var mixed $helperMethodResult */
$helperMethodResult = $block->getData('helper_method_result');
```

### arguments {#arguments}

`<arguments>` is a required container for `<argument>`. It does not have its own attributes.

```xml
<arguments>
    <argument name="css_class" xsi:type="string">header links</argument>
</arguments>
```

[page layout]: {{page.baseurl}}/frontend-dev-guide/layouts/layout-types.html#layout-types-page
[page configuration]: {{page.baseurl}}/frontend-dev-guide/layouts/layout-types.html#layout-types-conf
[generic layout]: {{page.baseurl}}/frontend-dev-guide/layouts/layout-types.html#layout-types-gen
[handle]: {{page.baseurl}}/frontend-dev-guide/layouts/layout-overview.html#layout-over-terms
[templates]: {{page.baseurl}}/frontend-dev-guide/templates/template-overview.html
[app/code/Magento/Theme/view/frontend/layout/default.xml]: {{site.mage2000url}}app/code/Magento/Theme/view/frontend/layout/default.xml
[app/code/Magento/Theme/view/frontend/templates/html/title.phtml]: {{site.mage2000url}}app/code/Magento/Theme/view/frontend/templates/html/title.phtml
[Layout file types]: {{page.baseurl}}/frontend-dev-guide/layouts/layout-types.html

