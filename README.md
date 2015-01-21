# ember-blog-introduction
A step-by-step guide to making a basic blog

---

Steps:

1.  [Creating Posts Page](#creating-posts-page)
2.  [Create Postid Page](#create-postid-page)
3.  [Create a Posts/new Page](#create-a-postsnew-page)
4.  [Handling Making New Posts](#handling-making-new-posts)

---

### Creating Posts Page

#### Create the project blog
```
ember new test-blog; cd test-blog
```

#### Scaffold Post's model and route
```
ember g resource posts
```
g is shorthand for generator. `resource` generates a model, route, tests, and template

#### Run ember server
```
ember server
```
ember server creates a watch to update minification

#### Create an Application Adapter
```
ember g adapter application
```
in app/adapters/application
```
import DS from 'ember-data';
export default DS.FixtureAdapter.extend();
```
We are making a global adapter for the application.
The adapters tell ember how it is suppose to access model data. For now, lets set it to the FixtureAdapter

#### Map the route to the model
in routes/posts.js:
```
export default Ember.Route.extend({
  model: function() {
    return this.store.find('post');
  }
});
```
Because we are fetching with ember-data, the model has a relationship that depends on this.store.find()

#### Define the Post Model
```
var Posts = DS.Model.extend({
  title: DS.attr('string'),
  description: DS.attr('string')
});

export default Posts;
```
We are defining the schema for the posts model.
We could do this with the generator, but its nice to go this route

#### Define Fixtures
```
Posts.reopenClass({
  FIXTURES: [
    {id: 1, title: 'hello world', 'description': 'world'},
    {id: 2, title: 'hello world 2', 'description': 'hello world 2'}
  ]
});
```

#### Make the PostIndex page
```
ember g template posts/index
```
in templates/posts/index
```
<ul>
   {{#each model}}
    <li>{{title}}</li>
   {{/each}}
</ul>
```

#### Update Template and Test
in templates/posts:
```
<ul>
     {{#each model}}
          <li>{{title}}</li>
     {{/each}}
</ul>
```
goto: http://0.0.0.0:4200/posts

### Create Post/:id Page

#### Scaffold the Post page 
```
ember g resource post
(don’t override the model)
```

#### Update router with post
in router.js
```
this.resource("post", { path: '/post/:post_id' });
```
This adds the `/post/:id` path

#### Update Post router
 routes/post
```
model: function(params) {
  return this.store.find('post', params.post_id);
  }
```

#### update link from posts to post
templates/posts/index
```
  {{#each model}}
    <li>
      {{#link-to 'post' this}} {{title}} {{/link-to}}
    </li>
  {{/each}}
```

#### Add content to the post page
in templates/post:
```
<h2>{{title}}</h2>
<p>{{description}}</p>
```


### Create a Posts/New Page

#### Generate a controller and route
```
ember g controller posts/new
ember g route posts/new
```
 
#### Add path to new page
in templates/posts/index
```
{{#link-to 'posts.new'}}New Post{{/link-to}}
```

#### Update the content on the posts/new page
in templates/posts/new
```
<h2>New Post</h2>
{{input type="text" placeholder="Title" value=title}}
{{textarea placeholder="content" value=description}}
<button {{action 'submit'}}>Submit</button>
```

#### Update the Controller to handle the submit action
in controller/posts/new
```
export default Ember.Controller.extend({
       actions: {
            submit: function(){
                 var post = this.store.createRecord('post', {
                      title: this.get('title'),
                      description: this.get('description')
                 });
                 post.save();
            }
       }
  });
```

### Handling making new posts

#### create posts mock 
```
ember g http-mock posts
```
Creates `/server/mocks/posts`

#### Update our adapater to RESTADAPTER
in adapters/application
```
export default DS.RESTAdapter.extend({
     namespace: 'api'
});
```

#### Update mocks to have an in memory store
in server/mocks/posts:
  copy/paste:
    https://gist.github.com/TheIronDeveloper/69de8b6273586f155d2b
    
#### Update the Controller to redirect on success
in controller/posts: 
```
export default Ember.Controller.extend({
  actions: {
    submit: function(){
      var post = this.store.createRecord('post', {
          title: this.get('title'),
          description: this.get('description')
        }),
        self = this;
      post.save().then(function(){
        self.transitionToRoute('posts');
      });
    }
  }
});
```

### Future ?
Add comments?
add an edit page?
discussion?
