Sublime Text 3 For Rails development
=====================================

References: https://mattbrictson.com/sublime-text-3-recommendations

Packages
---------

**Advance New File**
https://packagecontrol.io/packages/AdvancedNewFile

**All Autocomplete**
https://packagecontrol.io/packages/All%20Autocomplete

**Clipboard Manager**
https://packagecontrol.io/packages/Clipboard%20Manager

**DashDoc (very important in mac, to jumb to definition in Dash(documentation app))**
https://packagecontrol.io/packages/DashDoc

**DocBlockr**
https://packagecontrol.io/packages/DocBlockr

**Emmet (very important)**
https://packagecontrol.io/packages/Emmet

**GitGutter**
https://packagecontrol.io/packages/GitGutter

**SublimeLinter**
https://packagecontrol.io/packages/SublimeLinter


SETTINGS
--------

    {
    "auto_complete": true,
    "auto_complete_commit_on_tab": true,
    "copy_with_empty_selection": true,
    "ensure_newline_at_eof_on_save": true,
    "index_files": true,
    "rulers":
    [
    79
    ],
    "tab_size": 2,
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    }

**keymap goto definition **

    ⌥ opt ⌘ cmd d

**Ruby-specific word selection behavior**

When you double-click a word, or use any other word-related text selection commands, Sublime tries to be smart about where words begin and end. By default, it assumes punctuation is not part of the word. But in Ruby, punctuation can be part of method names: e.g. empty? and chomp!, among many others.

To tell Sublime to include those trailing ? and ! characters in word selections, add the following property to the Ruby syntax-specific settings. This has a side-effect of improving the Dash ``⌃ ctrl h` behavior as well!

    {
    "word_separators": "./\\()\"'-:,.;<>~@#$%^&*|+=[]{}`~"
    }
(To get to the Ruby settings, open a Ruby file in the editor and then navigate the menus to Sublime Text → Preferences → Settings – More → Syntax Specific – User.)

SNIPPETS AND COMMANDS
---------------------

**Singleton class snippet**

    <snippet>
      <content><![CDATA[
    class << ${1:self}
      $0
    end
    ]]></content>
      <tabTrigger>meta</tabTrigger>
      <scope>source.ruby</scope>
    </snippet>

KEY MAPPINGS
--------------

NOTE: here we are also changing default `ctrl + v` to paste and indent automatically just like doing `ctrl + shift + v`

    [
      { "keys": ["super+\\"], "command": "toggle_side_bar" },
      { "keys": ["super+shift+\\"], "command": "reveal_in_side_bar"},
      { "keys": ["super+shift+w"], "command": "close_others" },

      { "keys": ["super+backspace"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },

      { "keys": ["super+shift+r"], "command": "goto_symbol_in_project" },
      { "keys": ["super+alt+r"], "command": "goto_definition" },

      { "keys": ["super+shift+t"], "command": "reopen_last_file" },

      { "keys": ["super+x"], "command": "clipboard_manager_cut" },
      { "keys": ["super+c"], "command": "clipboard_manager_copy" },
      { "keys": ["super+v"], "command": "clipboard_manager_paste", "args": { "indent": true } },
      { "keys": ["super+ctrl+v"], "command": "clipboard_manager_paste", "args": { "indent": false } },
      { "keys": ["super+shift+v"], "command": "clipboard_manager_previous_and_paste" },
      { "keys": ["super+alt+ctrl+v"], "command": "clipboard_manager_choose_and_paste" },
    ]


CUSTOM THEME AND ICON
---------------------

**The “Primer” theme**
https://packagecontrol.io/packages/Theme%20-%20Primer

    {
      "color_scheme": "Packages/User/SublimeLinter/primer.light (SL).tmTheme",
      "theme": "Primer.sublime-theme",
      "theme_primer_sidebar_font_large": true,
      "theme_primer_sidebar_tree_small": true,
      "theme_primer_tabs_font_large": true
    }


Custom icon
-----------
Download from: https://dribbble.com/shots/1582459-Sublime-Text-Icon-for-Yosemite

To install the icon, first locate the Sublime Text application in the Finder and press ⌘ cmdi. Then drag and drop Rafael’s Sublime Text.icns file onto icon section of the Get Info window. If Sublime is in your Dock, you may need to restart the Dock process for the new icon to appear.
