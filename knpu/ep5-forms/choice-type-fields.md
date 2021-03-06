# Choice Type Fields

Coming soon...

Okay, we're going to tackle one of the most annoying things in Symfonys farm system
and I. my hope is that we can make it painless so that it works really well. Log in
as an Admin admin too@thespace.com, password engage,

and then manually head over to the `/admin/article` page. Here's the goal on this
form, I want to create two new dropdowns, a location dropdown so you can choose where
and the galaxy you are, but then for some of those drop downs for when you select one
of those choices, a second dropdown will appear and the options in that second
dropdown and will be different based on the first dropdown if somebody called
dependent foreign fields and it's actually pretty tricky to do so. We are going to
tackle that. First. Let's add, let's add the first new field. Find a terminal and run

```terminal
php bin\console make:entity
```

Let's modify the `Article` entity and create a new field
called `location`. It's just gonna be a simple `string` field and I'm going to make it.
Yes, `nullable` one on the database. So this new location is optional and let's just add that
one field for now. Next run 

```terminal
php bin\console make:migration
```

and then open `Migrations\` directory and check out that new migration file. Yep. 
No surprises. Looks perfect. Soon. Moved back on run 

```terminal
php bin\console doctrine:migrations:migrate
```

Perfect.

Open up are formed of class. For this `ArticleFormType`. Let's add this new field. So
I'll say ad it's going to be called `location` and we're going to make. We're going to
use the `ChoiceType` to make this a dropdown. To use the dressed up, we just need to
add a `choices` option. In here. I'm mad just three choices. 
`'The Solar System' => 'solar_system'`

`'Near a star' => 'star'` and `'Interstellar Space' => 'interstellar_space'`. We
haven't used the `ChoiceType` directly yet, but you only when you use the choice type,
the key in the choices is what's actually going to be displayed and the value over
here is actually what's going to be set onto our entities. So this will be the field,
the value that you're actually going to see in the database. So when we say this at
the bottom, I also want to put `'required' => false` and the reason for that is this
controls the html5 required attribute and unfortunately as soon as we add the add
the field type here form field type guessing stops and so it normally would guess the
required as you correctly by looking at the doctrine metadata, but it's going to not,
it's not going to guess that property at all and it's going to defaults required.
True. So basically I don't want the html five required attribute in this case, so I
have to say required false. Alright? So if we go back now and refresh it will work,
but in a bit of a surprising way, location is shows up all the way in the bottom of
the form. That's strange. The reason

is that we forgot to render it. Open `templates\article_admin\_form.html.twig`. 
You can see we're not rendering that field yet and when you forget
to render a field `{{ form_end() }}` actually renders it for you. It's actually kind of a nice
reminder that you forgot it. Course. We don't want it to render all the way at the
bottom like that. So instead, let's render `{{ form_row(articleForm.location) }}`
And the other thing is, is that I want to control the, um, a `placeholder`
value there. So that's also before we try this, add a `'placeholder' => 'Choose a location'`

Cool. So for refresh, it looks nice, Newfield, placeholder, and if we tried that,
that would save. Okay. So let's add the second field that we want here. So the idea
is that if we choose the solar system or a star, I want a second dropdown to be shown
that lists all of the planets. If you chose solar system and if you chose to start
with some of the most popular star systems that they're in, and we're going to store
that on his second field. So in your terminal, go back to 

```terminal
php bin\console make:entity
```

once again to update the `Article` on. And this time we're gonna create a new
field called `specificLocationName` that would, that would store, for example,
like the planet earth or Mars will make this `nullable` on the database again. So that it's
optional and we'll just add that one field. Of course we'll run, 

```terminal-silent
php bin\console make:migration
```

Then pretty confident

that that won't contain anything, anything extra. So we'll execute that migration.

```terminal-silent
php bin\console doctrine:migrations:migrate
```

Perfect.

And then we'll run over here when a copy of that location

at field called `specificLocationName` for the place to say where exactly and for the
choices, oh, this is where things get interesting. I'm just going to put a to do for
now. Then in our form template, let's render this right below foreign location. We'll
say form `specificLocationName`. Now if you refresh this, no surprise, it's going to
work. Here's our location and here's our specific location name, but this is not how
we want this to work. If solar system is selected, I want this to contain a list of
planets. If near stars selected, this should be a different list of stars and if
interstellar space is selected, I don't want this field to be in the form at all, was
going to be any drop down and there's not supposed to be any specific location. Name.
The way to solve this as a combination of a form, events and JavaScript, let's jump
into the forum events part next, that this field is dynamically added to our form
based on the submitted data.