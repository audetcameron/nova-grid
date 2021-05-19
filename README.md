This was a MR fix for a nova update issue Based on this package that was dropped by the author https://packagist.org/packages/jobcerto/nova-grid.

The package is now maintained at https://github.com/nibri10/nova-grid


# Grid Layout tool for Laravel Nova


This is a tool for Laravel's Nova administrator panel that allows you to create grids-based-layout for your resources.

## Installation

You can install the package via composer:

```
composer require jobcerto/nova-grid
```

Then, you will need to register the tool within the `NovaServiceProvider.php`:

```php

use Jobcerto\NovaGrid\NovaGrid;

...

/**
 * Get the tools that should be listed in the Nova sidebar.
 *
 * @return array
 */
public function tools()
{
    return [
        // other tools
        new NovaGrid,
    ];
}
```

## Using this tool

* After set the tool, all your fields now have access to an meta property `size` and the value is any of tailwind classes for sizes.

## EXAMPLE

```php

public function fields(Request $request)
    {
        return [
            ID::make()->sortable(),

            Text::make('Name')
                ->size('w-1/3'),

            Select::make('Type')
                ->size('w-1/3')
                ->options(UserType::toSelectArray()),

            Text::make('Email')
                ->size('w-1/3')
                ->sortable()
                ->rules('required', 'email', 'max:254')
                ->creationRules('unique:users,email')
                ->updateRules('unique:users,email,{{resourceId}}', 'sometimes'),

            Password::make('Password')
                ->size('w-1/3')
                ->onlyOnForms()
                ->creationRules('required', 'string', 'min:6')
                ->updateRules('nullable', 'string', 'min:6', 'sometimes'),

            InlineSelect::make('Status')
                ->size('w-1/3')
                ->options(Setting::userStatuses())
                ->inlineOnIndex()
                ->fillUsing(function ($request, $model) {
                    $model->applyStatus($request->status);
                }),

            BelongsTo::make('Office')
                ->size('w-1/2')
                ->nullable(),
        ];
    }
```
Keep in mind that this is an 15 minutes work tool, so use carefully
