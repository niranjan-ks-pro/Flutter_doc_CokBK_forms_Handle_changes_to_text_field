# Flutter_doc_CokBK_forms_Handle_changes_to_text_field
 https://docs.flutter.dev/cookbook/forms/text-field-changes#interactive-example
Handle changes to a text field
==============================

1.  [Cookbook](https://docs.flutter.dev/cookbook)
2.  [Forms](https://docs.flutter.dev/cookbook/forms)
3.  [Handle changes to a text field](https://docs.flutter.dev/cookbook/forms/text-field-changes)

In some cases, it's useful to run a callback function every time the text in a text field changes. For example, you might want to build a search screen with autocomplete functionality where you want to update the results as the user types.

How do you run a callback function every time the text changes? With Flutter, you have two options:

1.  Supply an `onChanged()` callback to a `TextField` or a `TextFormField`.
2.  Use a `TextEditingController`.

[](https://docs.flutter.dev/cookbook/forms/text-field-changes#1-supply-an-onchanged-callback-to-a-textfield-or-a-textformfield)1\. Supply an `onChanged()` callback to a `TextField` or a `TextFormField`
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The simplest approach is to supply an [`onChanged()`](https://api.flutter.dev/flutter/material/TextField/onChanged.html) callback to a [`TextField`](https://api.flutter.dev/flutter/material/TextField-class.html) or a [`TextFormField`](https://api.flutter.dev/flutter/material/TextFormField-class.html). Whenever the text changes, the callback is invoked.

In this example, print the current value of the text field to the console every time the text changes.

content_copy

```
TextField(
  onChanged: (text) {
    print('First text field: $text');
  },
),
```

[](https://docs.flutter.dev/cookbook/forms/text-field-changes#2-use-a-texteditingcontroller)2\. Use a `TextEditingController`
-----------------------------------------------------------------------------------------------------------------------------

A more powerful, but more elaborate approach, is to supply a [`TextEditingController`](https://api.flutter.dev/flutter/widgets/TextEditingController-class.html) as the [`controller`](https://api.flutter.dev/flutter/material/TextField/controller.html) property of the `TextField` or a `TextFormField`.

To be notified when the text changes, listen to the controller using the [`addListener()`](https://api.flutter.dev/flutter/foundation/ChangeNotifier/addListener.html) method using the following steps:

1.  Create a `TextEditingController`.
2.  Connect the `TextEditingController` to a text field.
3.  Create a function to print the latest value.
4.  Listen to the controller for changes.

### [](https://docs.flutter.dev/cookbook/forms/text-field-changes#create-a-texteditingcontroller)Create a `TextEditingController`

Create a `TextEditingController`:

content_copy

```
// Define a custom Form widget.
class MyCustomForm extends StatefulWidget {
  const MyCustomForm({super.key});

  @override
  State<MyCustomForm> createState() => _MyCustomFormState();
}

// Define a corresponding State class.
// This class holds data related to the Form.
class _MyCustomFormState extends State<MyCustomForm> {
  // Create a text controller. Later, use it to retrieve the
  // current value of the TextField.
  final myController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is removed from the
    // widget tree.
    myController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next step.
  }
}
```

info Note: Remember to dispose of the `TextEditingController` when it's no longer needed. This ensures that you discard any resources used by the object.

### [](https://docs.flutter.dev/cookbook/forms/text-field-changes#connect-the-texteditingcontroller-to-a-text-field)Connect the `TextEditingController` to a text field

Supply the `TextEditingController` to either a `TextField` or a `TextFormField`. Once you wire these two classes together, you can begin listening for changes to the text field.

content_copy

```
TextField(
  controller: myController,
),
```

### [](https://docs.flutter.dev/cookbook/forms/text-field-changes#create-a-function-to-print-the-latest-value)Create a function to print the latest value

You need a function to run every time the text changes. Create a method in the `_MyCustomFormState` class that prints out the current value of the text field.

content_copy

```
void _printLatestValue() {
  print('Second text field: ${myController.text}');
}
```

### [](https://docs.flutter.dev/cookbook/forms/text-field-changes#listen-to-the-controller-for-changes)Listen to the controller for changes

Finally, listen to the `TextEditingController` and call the `_printLatestValue()` method when the text changes. Use the [`addListener()`](https://api.flutter.dev/flutter/foundation/ChangeNotifier/addListener.html) method for this purpose.

Begin listening for changes when the `_MyCustomFormState` class is initialized, and stop listening when the `_MyCustomFormState` is disposed.

content_copy

```
@override
void initState() {
  super.initState();

  // Start listening to changes.
  myController.addListener(_printLatestValue);
}
```

content_copy

```
@override
void dispose() {
  // Clean up the controller when the widget is removed from the widget tree.
  // This also removes the _printLatestValue listener.
  myController.dispose();
  super.dispose();
}
```
