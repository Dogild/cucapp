# Welcome to Cuccap

## Introduction

Cucapp is an interface between Cucumber (see: http://cukes.info) and Cappuccino.

The Cappuccino application is served via thin and a small piece of code is injected.
This code connects back to your Cucumber script via AJAX requests

This code is based heavily on Brominet (see: http://github.com/textarcana/brominet)

Original Concept and Developer: Daniel Parnell (see: https://github.com/dparnell)


## Installation

To get started, download the current version of Cuccap:

    $ git clone git://github.com/cappuccino/cucapp.git (git)

Then install cucapp on your system:

    $ gem build cucapp.gemspec && gem install cucapp

The following gems are going to be installed:

- Cucumber
- Thin
- Nokogiri
- JSON
- Launchy


## Usage

#### Environement Variables

You can set different env variables to configure Cucapp :

Cucapp provides a set of environment variables :

* `$CUCAPP_PORT` allows you to specify the port used by the Thin server.
* `$CUCAPP_APPDIRECTORY` allows you to specify where the Cappuccino application is located.
* `$CUCAPP_BUNDLE` allows you to specify if you want to use the compiled version of Cucapp.
* `$CUCAPP_APPLOADINGMODE` allows you to specify which version (`build` or `debug`) of your Cappuccino application you want to test.

#### Global variable

The global variable `$url_params` (which is a dictionary) allows you to specify URL params between each scenarios (need to be changed in the hooks).

#### Categories

- `CPResponder+CuCapp.j` contains a category of `CPResponder`. It adds the method `-(void)setCucappIdentifier:`. This `cucappIdentifier` can be used to identify the control with its XPath. You need to include this category in your Cappuccino application to use cucappIdentifiers. With that, you can use a xpath such as `//CPButton[cucappIdentifier='cucappIdentifier-button-bar-add']`. This category contains also a CLI mode for Cucapp, more informations below.

- `Cucumber+Extensions.j` will be loaded (optionally) by Cucapp when launching Cucumber. It allows you to add new Cappuccino methods needed for your own tests (for instance a method to check the color of a CPView). This file has to be located in `features/support/Cucumber+Extensions.j`.

#### Features

Cucapp provides a set of basic methods who can be called from Cucumber (take a look at encumber.rb and Cucumber.j). You should mainly used the following methods :

```ruby
    def simulate_keyboard_event charac, flags
    def simulate_keyboard_events string, flags
    def simulate_left_click xpath, flags
    def simulate_left_click_on_point x, y, flags
    def simulate_double_click xpath, flags
    def simulate_double_click_on_point x, y, flags
    def simulate_dragged_click_view_to_view xpath1, xpath2, flags
    def simulate_dragged_click_view_to_point xpath1, x, y, flags
    def simulate_dragged_click_point_to_point x, y, x2, y2, flags
    def simulate_right_click xpath, flags
    def simulate_right_click_on_point x, y, flags
    def simulate_scroll_wheel xpath, deltaX, deltaY, flags
    def simulate_mouse_moved_on_point x, y, flags
````

Cucapp automatically simulates several CPMouseMoved events between two simulated events generated by the tester. Like a real user does.

Example of a step:

 ```ruby
I want to fill a form and send the informations do
  app.gui.wait_for                    "//CPTextField[cucappIdentifier='field-name']"
  app.gui.simulate_left_click         "//CPTextField[cucappIdentifier='field-name']", []
  app.gui.simulate_keyboard_event     "a", [$CPCommandKeyMask]
  app.gui.simulate_keyboard_event     $CPDeleteCharacter, []
  app.gui.simulate_keyboard_events    "my_new_name", []
  app.gui.simulate_keyboard_event     $CPTabCharacter , []
  app.gui.simulate_keyboard_events    "my_new_family_name_", []
  app.gui.simulate_left_click         "//CPButton[cucappIdentifier='button-send']", []
end
```

The rest is pure Cucumber, don't hesitate to take a look at their website ;) (see: http://cukes.info)

#### CLI

Cucapp contains a CLI mode. To use it, make sure your application import the category `CPResponder+CuCapp.j` and open the javascript console of your browser.

To load the CLI mode, you need to call the function `function load_cucapp_CLI(path)`. The path argument represents the path to the file `Cucumber.j`. By default this is set to `../../Cucapp/lib/Cucumber.j`. Once you have loaded the CLI, you can use the following methods :

``` javascript
function simulate_keyboard_event(character, flags)
function simulate_keyboard_events(string, flags)
function simulate_left_click_on_view(aKey, aValue, flags)
function simulate_right_click_on_view(aKey, aValue, flags)
function simulate_double_click_on_view(aKey, aValue, flags)
function simulate_left_click_on_point(x, y, flags)
function simulate_right_click_on_point(x, y, flags)
function simulate_double_click_on_point(x, y, flags)
function simulate_dragged_click_view_to_view(aKey, aValue, aKey2, aValue2, flags)
function simulate_dragged_click_view_to_point(aKey, aValue, x, y, flags)
function simulate_dragged_click_point_to_point(x, y, x1, y2, flags)
function simulate_mouse_moved_on_point(x, y, flags)
function simulate_scroll_wheel_on_view(aKey, aValue, deltaX, deltaY, flags)
function find_cucappID(cucappIdentifier)
```

For example, to simulate a left click on a button with a `cucappIdentifier` set to "button-login", you need to do:

``` javascript
load_cucapp_CLI()
simulate_left_click_on_view(“cucappIdentifier, “button-login", [])
```

## Demo

A full demo of what Cucapp can do is available [here](https://github.com/Dogild/Cucapp-demo).