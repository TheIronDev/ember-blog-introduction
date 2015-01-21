# ember-blog-introduction
A step-by-step guide to making a basic blog


|Create blog|
|---------|
| ember new test-blog; cd test-blog  |

|Scaffold Post's model and route|
|---------|
| ember g resource posts  |
| g is shorthand for generator. `resource` generates a model, route, tests, and template |

| Run ember server |
|---------|
| ember server  |
| ember server creates a watch to update minification |

| Create an Application Adapter |
|---------|
| ember g adapter application  |
| import DS from 'ember-data';<br/>export default DS.FixtureAdapter.extend();|
| ember server creates a watch to update minification |
| We are making a global adapter for the application. |
| The adapters tell ember how it is suppose to access model data. For now, lets set it to the FixtureAdapter |

to be continued.. (sleep)
