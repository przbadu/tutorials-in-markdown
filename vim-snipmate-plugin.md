If you are from Sublime Text 2/3 Or TextMate user, then you must be familier with their
snipmate(or autocompletion) feature. For example, if you type html and then press ``Tab``
key in Sublime Text it will automatically generate below html tags:

```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>
    </title>
  </head>
  <body>

  </body>
  </html>
```

Now, what if we want to mimic this functionality in vim editor. Thanks to opensource community, 
`vim plugin` available for us. Please visit http://code.tutsplus.com/tutorials/vim-essential-plugin-snipmate--net-19356 for more details.

**Installation**

Open `.vimrc` file to add `Vundle Plugin` in it

    $ vi ~/.vimrc

Then, add below line in it:

    Plugin ‘msanders/snipmate.vim'

And finally, ``VundleInstall`` for installing that snipmate.vim plugin:

    :VundleInstall

Once your snipmate plugin is installed, you can confirm that by opening up new terminal or reloading current terminal and open ``example.html`` file. And then type ``html`` and then press ``Tab`` key in the keyword. If that automatically add ``<html> </html>`` tag for you, then you have successfully installed ``SnipMate`` plugin for your vim editor.

**NOTE: you can also try SnipMate for other PLs please check https://github.com/msanders/snipmate.vim/tree/master/snippets for supported languages. for example html, ruby, php, python, shell, etc**


**Customize**

Now, that we successfully installed ``SnipMate`` plugin, it would be great if we can customize it as per our choice. 

For example, I am a ``Ruby On Rails`` developer and by default, I didn't see support for ``html.erb`` tags. So It would be great if I can add them myself. And we can do that easily:

Check ``snippets`` directory for snipmate.vim (location to this directory depends on your installation type. Example, if you have installed snipmate using `Vundle` then check it under `~/.vim/bundle/snipmate.vim/snippets/`) and you will see the list of suported snippets by default. Which also includes `html` and `ruby` by default but not `eruby`.

So, our next target is to create some example snippets for ``eruby`` tags. looking at the files strucutre, we can see that all snippets are started with their name.snippets so looks like we need to add ``eruby.snippets``

for making it simpler, just duplicate ``html.snippets`` with the file name ``eruby.snippets`` so that we can atleast access html tags inside ``html.erb`` templates.

    $ cp html.snippets eruby.snippets

Now, we have mimic all the html snippets for ``erb`` files. Let's check, whether it is working or not:

    $ vi test.html.erb

type, `html` and press `TAB` and if it creates ``<html></html>`` tag for you, you have successfully mimiced ``eruby`` with ``html`` snippets.

Now what about actual embedded ruby snippets. Well, either we have to create those snippets ourself, or we can try some other developers snippets which they have already written and customize as per our need.

check https://github.com/scrooloose/snipmate-snippets this for more details.

But for now, we will create a simple yet most commonly used erb snippet. that is for creating ``form_tag``

1) open your ``eruby.snippets`` file and add below snippets:

    snippet form_for
            <%= form_for @${1:model} do |f| %>
              ${2}
            <% end %>

Now, reload `test.html.erb`, and then type ``form_for`` and press ``TAB`` key. You will see nice snippets which will autocomplete your form tag and ask you for model name.

Isn't it great!!

Finally, lets add some Lorem Ipsum text when user type `lorem` and press `Tab` key just like Sublime Text editor does:

You can add this functionality for `eruby.snippets`, `html.snippets`, `slim.snippets` and others where you think this can be helpful:

for now we will take example for `eruby.snippets` so add below snippets for it:

    snippet lorem
            Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
            tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
            quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
            consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
            cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
            proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Save it and again reload `test.html.erb` file and type `lorem` and press `Tab` your lorem epsum text will be automatically generated.

That's great :) ...


