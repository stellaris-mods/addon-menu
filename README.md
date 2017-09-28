# How-to

To use the generic addon features menu, there are only a few steps you need to do.

1. Copy `addonmenu.txt` to your `events/` folder.

2. Search the file for CHANGEME to find places you must add stuff (there are two spots).

3. Add these keys to any one of your localisation files, anywhere. You can not change the text in these and expect your changes to show in the menu. If you want them changed, they need to be changed in all mods that use the addon.
```
addonMenu.title: "Addon Features"
addonMenu.desc: "Welcome! In this menu you can find lots of features to enable/disable before game start. You are the only player that has access to this menu, so please choose carefully - and most likely you should confer with any other players about which features to enable or disable.\n\n§PIt's important that you take the time to go through the menus. No other player can see this menu.§!"
addonMenu.done: "§GApply features and close menu§!"
addonMenu.back: "§Y<<< Back§!"
```
4. Add these in your on_action file (change # with your version):
```
on_game_start = {
	events = {
		# Set the same global flag you add in the addonMenu.20 event
		set_my_own_has_global_flag.1
		# Adds +1 users to addon menu
		addonMenu.10
	}
}
on_press_begin = {
	events = {
		# Opens addon menu if there is more than 1 user
		addonMenu.20
		# Same event you run the button you add to addonMenu.20
		my_own_mod_menu.1
	}
}
```
5. Use this code in your main menu to return to the main Addon Features Menu. Of course you can use any `name` you want. The trigger makes sure that the option is only shown if there is more than one user of modmenu.
```
option = {
	name = "addonMenu.back"
	# Optionally default_hide_option = yes
	trigger = {
		has_global_flag = "inAddonMenu"
		has_global_flag = "addonMenuMultiple"
	}
	hidden_effect = { country_event = { id = addonMenu.20 } }
}
```
6. Use this code as your main menu event trigger - the event you run from `on_press_begin` (`my_own_mod_menu.1`).
```
trigger = {
	is_same_value = event_target:addonMenuCountry
	OR = {
		has_global_flag = "inAddonMenu"
		NOT = { has_global_flag = "addonMenuMultiple" }
	}
}
```
7. Add this code in your main menu to apply your effects. Of course you can use any `name` you want (and `custom_tooltip` if you want). The point is that the trigger only shows the option if there are no other addons using the addon features menu. Remember `my_own_mod_menu.1` should be the same event you use in `addonMenu.20` in the `"addonMenu.done"` button there.
```
option = {
	name = "addonMenu.done"
	trigger = {
		NOT = { has_global_flag = "addonMenuMultiple" }
	}
	hidden_effect = { country_event = { id = my_own_mod_menu.1 } } # CHANGEME
}
```

8. ???

9. Profit!

But seriously, step 9 is actually: Make damn sure that no buttons in your menu (`my_own_mod_menu.1`) close the window. All your buttons should either lead back to `my_own_mod_menu.1` or `addonMenu.20`. Except for the one you added in step 7. If any of your buttons close the window, you ruin it for the other addons using modmenu.

# Done!

If you do add this file to your mod and add options, other users of the file must be informed, so that they can update their mods.

So please inform folk on github, Discord, or by email at folk@folk.wtf. If you are a user of this addon features menu, you should probably also Watch the Github repository to be notified of any changes.
