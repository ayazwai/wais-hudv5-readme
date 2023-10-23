# WAIS-HUDV5 
- #### Author: Ayazwai#3900
- #### Linkedin: [Ayaz Ekrem](https://www.linkedin.com/in/ayaz-ekrem-770305212/)
- #### Instagram: [Ayaz Ekrem](https://www.instagram.com/ayaz.ekremm/)
- #### Discord Server: [Wais Development](https://discord.com/invite/YanMPNg8Zn)
- ### Sold exclusively at [0resmon](0resmon.tebex.io) tebex or [Wais Development](https://discord.com/invite/YanMPNg8Zn) Discord.

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
files({
    'html/index.html',
    'html/sounds/*.ogg',
})
```

- ### Ox_inventory integration for ESX

- - For those who use ox_inventory, there is a step to be done.
- - Go to `es_extended\server\classes\overrides` and open `oxinventory.lua` file. 
- - You should find the `syncInventory` function and add this event like the photo below. Event ` TriggerClientEvent('wais:checkInventory', self.source, items) `
- - ![Event view](https://media.discordapp.net/attachments/1035485961217384488/1159986975667933204/image.png?ex=6533050e&is=6520900e&hm=d21d19fa9fb271aefb9097374e80e0c7948bb98a4ca0ca0e4ebde68b79439b8a&=&width=652&height=441)

- ### Framework Settings
- - You can change the framework variable to `esx` or `qbcore` depending on the type you use

- - In the ResourceName section, you should write the file name of the framework. Usually these are [es_extended, qb-core, nc-core blah blah blah]

- - If your infrastructure is new, the `NewCore` variable must be `@true`, if not, it must be `@false`. ESX Legacy and QBCore are newcore 

- - If you are using SharedEvent old core, you can find and paste the shared event from any script or from the infrastructure itself. This is important.

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

---

## How do I activate the map only in the vehicle?
* Open the config file and you will see a variable called ***Config.ShowMapOnlyInTheCar***. If you change this variable to `true @boolean`, the map will only work when you are in the car.

## How can I add custom weapon photos?

- Find the weapon you want to change the photo of in the Config.Weapons variable, if it does not exist, add it like the other examples. 
- After finding the weapon, add the following variable `["image"] = "weapon-file-name"`
- The file name of the weapon should appear where Filename is written.
- Put the weapon photo in the `Public > weapons` folder.

## How to activate Stress?
* If you change the variable named ***Config.StressSystem*** in the config file to `true @boolean`, you will see that the necessary settings for stress and stress are now active in hud. Note that for ESX this system will only reflect the data, there are no lines of code to increase or decrease stress. There is such a file for QB, you can download it from here. [QBCore Stress System](https://cdn.discordapp.com/attachments/1035485961217384488/1099749164302205088/qb-stress.rar)

## What is ***Config.RefreshTimes***?
* This regeneration time affects resmon. If you increase the number to 200 or 500, health, armor, food, thirst, stamina and stress information will be updated a little later. Increasing the number will cause the resmon to decrease and decreasing the number will cause it to increase. 

## How do I adapt Hud to a voice system?

* You can run hud with ***SALTYCHAT*** or ***PMA-VOICE*** thanks to this table structure ***Config.VoiceSettings*** in the config file.

## What should I do if I get such a crash
![Saltychat crash](https://forum.cfx.re/uploads/default/original/4X/e/8/6/e86d3cc7d96d4deb74f6b50601d43ebe2239af20.png)
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
