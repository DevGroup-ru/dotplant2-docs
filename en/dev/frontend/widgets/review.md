# Widget reviews

Reviews widget generates the specified location on your website a list of available reviews and the form to add a new comment.
The administrative part of the site in the section displays a list of reviews Add review.
Added review appears on the site only after the administrative part of it will be marked `approved`, which will mean its approval a site moderator.
In addition to changing the status, a comment can be deleted. No other manipulations reviews across the administrative part of the site to make it is impossible.

For authorized users when you add a new review shows 2 fields default phone and text. Text field is optional.
For unauthorized user displays four fields:

- Username - mandatory,
- Email - optional,
- Telephone - optional,
- Text - mandatory.

You can also add any number of fields to the comment ask them to form.

### Minimum call widget is as follows:

```php
\app\modules\review\widgets\ReviewsWidget::widget(
	[
		'model' => $model,
		'formId' => 1,
		'additionalParams' => [
			'model' => $model,
		],
	]
)
```

#### Call Settings:

- `formId` - form ID. Integer
- `model` - a model of the current product\category\page. ActiveRecord inherited
- `additianalParams` - additional parameters needed to build the correct URL. Array key => value (Optional)
- `ratingGroupName` - the name of the group rankings. String (Optional)
- `viewFile` - path to the presentation file. String (Optional)
- `registerCanonical` - Canonical add a link in the HEAD section. Boolean (Optional)
- `useCaptcha` - display captcha. Boolean (Optional)

### Example of review in the product page:

```php
\app\modules\review\widgets\ReviewsWidget::widget(
	[
		'model' => $model, //  Product Model
		'formId' => 1, // ID form
		'additionalParams' => [
			'model' => $model, // Additional options
		],
	]
)
```
```