# WAIS-HUDV5 
- #### Author: Ayazwai#3900
- #### Linkedin: [Ayaz Ekrem](https://www.linkedin.com/in/ayaz-ekrem-770305212/)
- #### Instagram: [Ayaz Ekrem](https://www.instagram.com/ayaz.ekremm/)
- ### Sold exclusively at [0resmon](0resmon.tebex.io) tebex.

# How to set it up

- Do not change the name of the script
- Put the script in the resources folder, do not use any folderisation.
- Prefer to start manually in server.cfg as follows.

```
ensure wais-compatibility
ensure interact-sound
ensure wais-hudv5
```

- The [interact-sound](https://github.com/plunkettscott/interact-sound) script is one of the requirements for hudv5 to work.
- After downloading interact sound, go to the sounds folder in hudv5 and copy the sounds.
- then drop the copied sound files into the `interact-sound > html > sounds` folder. 
- Open the interact-sound fxmanifest file and change the `files` variable to 

```
files {
    'client/html/index.html',
    'client/html/sounds/*.ogg'
}
```

- ### Ox_inventory integration for ESX

- - For those who use ox_inventory, there is a step to be done.
- - Go to `es_extended\server\classes\overrides` and open `oxinventory.lua` file. 
- - You should find the `syncInventory` function and add this event like the photo below. Event ` TriggerClientEvent('wais:checkInventory', self.source, items) `
- - ![Event view](https://i.imgur.com/Si14Ld0.png)

- ### Framework Settings
- - You can change the framework variable to `esx` or `qbcore` depending on the type you use

- - In the ResourceName section, you should write the file name of the framework. Usually these are [es_extended, qb-core, nc-core blah blah blah]

- - If your infrastructure is new, the `NewCore` variable must be `@true`, if not, it must be `@false`. ESX Legacy and QBCore are newcore 

- - If you are using SharedEvent old core, you can find and paste the shared event from any script or from the infrastructure itself. This is important.

### Money adaptation for "OLD ESX" or "CHEZZA INVENTORY"
- Go to es_extended > server > classes > player.lua
- And find the functions below and add the events as done

```
	self.setMoney = function(money)
		money = ESX.Math.Round(money)

		if money >= 0 then
                    	self.player.setMoney(money)
            	    	TriggerEvent('wais:updateMoney', self.source)
		end
	end

	self.addMoney = function(money)
		money = ESX.Math.Round(money)

		if money >= 0 then
			self.player.addMoney(money)
			TriggerEvent('wais:updateMoney', self.source)
		end
	end

	self.removeMoney = function(money)
		money = ESX.Math.Round(money)

		if money > 0 then
			self.player.removeMoney(money)
			TriggerEvent('wais:updateMoney', self.source)
		end
	end
```

---

## How can I change the settings to default as I want?
- Enter the html file and find the public file
- Open the file named DefaultConfig.json in the Public file, then configure it as you wish.
- Below are the values that can be written
- The changes you make here will be active for the person who enters the server for the first time or resets the settings

```
    hudType: "newest", -- old, new or newest
    carhudType: "new", -- old, new or newest
    mapType: "squared", -- rounded or squared
    speedType: "kmh", -- kmh or mph
    hidedElements: {
        serverInfo: false, -- If you make any data true, it will not appear by default.
        cash: false,
        bank: false,
        job: false,
        weapon: false,
        keybinds: false,
        location: false,
    },
    elementsColors: {
        primary: "#67C8FF", -- You can set the default colours you want here.
    
        health: "#D35959",
        armour: "#5991D3",
        hunger: "#9D61FF",
        thirst: "#77FF61",
        stamina: "#C7C7C7",
        stress: "#FF61C9",

        carhud: "#67C8FF",
    },
```

## How do I (remove, delete or hide) elements
- Follow the file path wais-hudv5 / html / public / DeletedElements.json and open the file.
- Change the `false` variable next to the content you want to remove completely to `true` and then restart the script.

```
{
    "serverInfo": true,
    "id": false,
    "players": false,
    "clockNdate": false,
    "cash": false,
    "bank": false,
    "job": false,
    "job2": false,
    "gang": false,
    "weapons": false,
    "keybinds": false
}
```

---

## How do I activate the map only in the vehicle?
* Open the config file and you will see a variable called ***Config.ShowMapOnlyInTheCar***. If you change this variable to `true @boolean`, the map will only work when you are in the car.

## How can I add custom weapon photos?

- Find the weapon you want to change the photo of in the Config.Weapons variable, if it does not exist, add it like the other examples. 
- After finding the weapon, add the following variable `["image"] = "weapon-file-name"`
- The file name of the weapon should appear where Filename is written.
- Put the weapon photo in the `Public > weapons` folder.

## How to activate Stress?
* If you change the variable named ***Config.StressSystem*** in the config file to `true @boolean`, you will see that the necessary settings for stress and stress are now active in hud. Note that for ESX this system will only reflect the data, there are no lines of code to increase or decrease stress. There is such a file for QB, you can download it from here. [QBCore Stress System](https://cdn.discordapp.com/attachments/1035485961217384488/1099749164302205088/qb-stress.rar?ex=65f728b9&is=65e4b3b9&hm=60883bc5f25dd0081c8674a4d8070cdddac221a9a812a6bed3c5ba7a1035e7e0&)

## What is ***Config.RefreshTimes***?
* This regeneration time affects resmon. If you increase the number to 200 or 500, health, armor, food, thirst, stamina and stress information will be updated a little later. Increasing the number will cause the resmon to decrease and decreasing the number will cause it to increase. 

## How do I adapt Hud to a voice system?

* You can run hud with ***SALTYCHAT*** or ***PMA-VOICE*** thanks to this table structure ***Config.VoiceSettings*** in the config file.

## How Can I Hide Hud or Map?
* You can hide or unhide using these events.

```
    Client side events:
    "wais:hideHud",
    "wais:hideRadar"

    TriggerEvent("wais:hideHud", true or false) @boolean
    TriggerEvent("wais:hideRadar", true or false) @boolean
```

## What should I do if I get such a crash / Saltychat Range Error 
![Saltychat crash](https://i.imgur.com/uCo9OT8.png)
![Crash2](https://i.imgur.com/Bc561p2.png)
* You are getting this error because you are using *saltychat* and have not set the distances. If you want to know how to set them, follow these steps:

* Open **Saltychat/config.json** and find this veriable **"VoiceRanges"**. Enter the numeric values you see in this variable into hud just like in this example:

```
Config.VoiceSettings = {
    ["saltychat"] = {
        ["use"] = true,
        ["default"] = "3.0",
        ["ranges"] = {
            ["Range value. This value must be float (String)"] = {meter = Range Value (Number))}
            ["3.0"]  = {meter = 10},
            ["6.5"]  = {meter = 30},
            ["15.0"] = {meter = 100},
        }
    }
}
```

## Do not work in Carhud or Belt x Vehicle!
* If you want the belt or carhud to work in some vehicles, you must do the following:
* - Open the ***Config*** file and find the ***Config.BlackListVehicle*** variable.
* - Add a table without breaking the table structure as in the example:
```
Config.BlackListVehicle = {
    ['Vehicle Model Name'] = {
        ["carhud"] = true,
        ["belt"] = true
    }
}
```

## How Can I Use Notification?
* If you want to use this notification system in general, you can follow these ways.

## For ESX:
- - Open es_extended folder
- - Open client folder
- - Open functions.lua
- - Find the function called ***ESX.ShowNotification***

```
function ESX.ShowNotification(message, type, length)
    if Config.NativeNotify then
        BeginTextCommandThefeedPost('STRING')
        AddTextComponentSubstringPlayerName(message)
        EndTextCommandThefeedPostTicker(0, 1)
    else
        exports["esx_notify"]:Notify(type, length, message)
    end
end

Change to this:
--We added the Title argument 
--Remember to include the title argument in all scripts that use ESX.ShowNotification as much as possible, otherwise the message "Enter Text" will appear
function ESX.ShowNotification(message, type, length, title)
    if Config.NativeNotify then
        BeginTextCommandThefeedPost('STRING')
        AddTextComponentSubstringPlayerName(message)
        EndTextCommandThefeedPostTicker(0, 1)
    else
        TriggerEvent("wais:addNotification", type, title, message, length)
    end
end
```

## For QB:
- - Open qb-core folder
- - Open client folder
- - Open functions.lua
- - Find the function called ***QBCore.Functions.Notify***

```
function QBCore.Functions.Notify(text, texttype, length)
    if type(text) == "table" then
        local ttext = text.text or 'Placeholder'
        local caption = text.caption or 'Placeholder'
        texttype = texttype or 'primary'
        length = length or 5000
        SendNUIMessage({
            action = 'notify',
            type = texttype,
            length = length,
            text = ttext,
            caption = caption
        })
    else
        texttype = texttype or 'primary'
        length = length or 5000
        SendNUIMessage({
            action = 'notify',
            type = texttype,
            length = length,
            text = text
        })
    end
end

Change to this:

function QBCore.Functions.Notify(text, texttype, length)
    if type(text) == "table" then
        local ttext = text.text or 'Placeholder'
        local caption = text.caption or 'Placeholder'
        texttype = texttype or 'primary'
        length = length or 5000
        TriggerEvent("wais:addNotification", texttype, caption, ttext, length)
    else
        texttype = texttype or 'primary'
        length = length or 5000
        TriggerEvent("wais:addNotification", texttype, caption, text, length)
    end
end

```

Or normally using:

```
TriggerEvent("wais:addNotification",  type, "Title", "Message", 5000)
TriggerClientEvent("wais:addNotification", source, type, "Title", "Message", 5000)

--@title: string,
--@message: string,
--@type: string, [success, error, warning, info]
--@length: number
```

# How to use Job2
- You need to trigger the following event when the user has the 2nd job

```
---@table { name = "test", label = "test" } -- It must contain name and label variables.
---or just send name as argument
---@label -- string.

TriggerEvent('wais:set:job2', "JOB2 LABEL")
TriggerClientEvent('wais:set:job2', source, label or table)

```

# How to use Gang
- You need to trigger the following event when the user has the gang

```
---@table { name = "none", label = "none" } -- It must contain name and label variables.
--- Default name must be "none"

TriggerEvent('wais:set:gang', table)
TriggerClientEvent('wais:set:gang', source, table)

```
