
# 🚨 OGDev Studios - 911 Script (FiveM)

**Author:** TheOGDev / OGDev Studios
**Script Name:** `OG_Studios_911`
**Version:** 1.0
**Release Date:** 2025-04-30
**Framework:** FiveM

---

## 🔥 About This Script

The **OG\_Studios\_911** script is a **badass emergency call system** built for serious roleplay. It brings cinematic, real-time dispatching to your FiveM server by allowing players to call 911 and transmit detailed reports, including:

* 👤 **Caller name**
* 🧭 **Street-level location**
* 📝 **Custom report text**

Calls are automatically broadcast to responders with visual map blips and immersive formatting. Designed with customization and simplicity in mind, this system keeps your emergency RP smooth and impactful.

---

## 🧩 Features

✅ **Caller Name Detection**
✅ **Street Name Localization**
✅ **Custom Chat Suggestion**
✅ **Custom Radius Alert Blip**
✅ **Blip Duration Settings**
✅ **Dynamic Command Input**
✅ **Roleplay Immersion Focused**

---

## ⚙️ Configuration Options

At the top of the script:

```lua
chatSuggestion = true       -- Enable /911 chat suggestion (autocomplete in chat)
blipTime = 180              -- Time (in seconds) that the map blip stays active
blipRadius = 175.0          -- Radius of the area blip on the map (in meters)
```

Modify these to suit your server’s style or realism goals.

---

## 🚔 Example Call

A player types:

```
/911 There's a shooting at Legion Square. One person down.
```

Responders receive:

> 📞 **911 CALL FROM Alex Jackson**
> 🧭 **Location:** Legion Square
> 🗒️ **Report:** There's a shooting at Legion Square. One person down.

A large area blip appears at the location and lasts for `blipTime` seconds.

---

## 📂 Code Export – `client.lua`

```lua
-------------------------------------------------------------
-- Made by: TheOGDev / OGDev Studios
-- Name: OG_Studios_911
-- Desc: Just a 911 script
-- 
-- 04-30-2025
-------------------------------------------------------------

chatSuggestion = true -- Change to "False" if you would like to turn off the chat suggestion!
blipTime = 180 -- The amount of time the circle will stay on your map in seconds!
blipRadius = 175.0 -- Change this to change the radius of how big you want the 911 circle to be!

if chatSuggestion then
    TriggerEvent("chat:addSuggestion", "/911", "Call 911 for your emergency!", {
        { name = "Report", help = "Enter your report here!" }
    })
end

RegisterCommand('911', function(source, args)
    local name = GetPlayerName(PlayerId())
    local ped = GetPlayerPed(PlayerId())
    local x, y, z = table.unpack(GetEntityCoords(ped, true))
    local street = GetStreetNameAtCoord(x, y, z)
    local location = GetStreetNameFromHashKey(street)
    local msg = table.concat(args, ' ')

    if args[1] == nil then
        TriggerEvent('chatMessage', '^5San Andreas 911', {255, 255, 255}, 'What is the nature and location of your emergency?')
    end

    if args[1] ~= nil then
        TriggerServerEvent('call911', location, msg, x, y, z, name)
    end
end)

RegisterNetEvent('setBlip')
AddEventHandler('setBlip', function(name, x, y, z)
    blip = nil
    blips = {}

    local blip = AddBlipForRadius(x, y, z, blipRadius)
    SetBlipHighDetail(blip, true)
    SetBlipColour(blip, 3)
    SetBlipAlpha(blip, 128)

    local blip1 = AddBlipForCoord(x, y, z)
    SetBlipSprite(blip1, sprite)
    SetBlipDisplay(blip1, true)
    SetBlipScale(blip1, 0.9)
    SetBlipColour(blip1, 3)
    SetBlipAsShortRange(blip1, true)

    BeginTextCommandSetBlipName("STRING")
    AddTextComponentString("911 - " .. name)
    EndTextCommandSetBlipName(blip1)

    table.insert(blips, blip1)
    Wait(blipTime * 1000)

    for i, blip1 in pairs(blips) do 
        RemoveBlip(blip)
        RemoveBlip(blip1)
    end
end)
```

---

## 🚀 Installation Instructions

1. **Place the resource** into your `resources` folder.
2. Add the following line to your `server.cfg`:

   ```bash
   ensure OG_Studios_911
   ```
3. Start your server and you're ready to RP with dispatches like a pro.

---

## 🛠️ Future Improvements (Ideas)

* 🔔 Add sound effects when alerts are received.
* 📋 Log calls to a server log or external database.
* 🎨 Add UI/HTML popup for high-quality dispatch interface.
* 🔐 Role-based alert routing (e.g., only police get crime-related 911s).
* 🗺️ Integrate postal or zone-based location systems.

---

## 📞 Contact

For updates or support, visit:
**Discord:** *discord.gg/ogdevstudios*
**Website:** *Coming Soon*

