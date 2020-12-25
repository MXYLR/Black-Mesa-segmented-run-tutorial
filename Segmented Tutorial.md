# Black Mesa segmented run tutorial

## Black Mesa setup

If you know nothing about black mesa speedrun, see : https://www.speedrun.com/bms/guide/qhn4v

## What is segmented

## Segmented setup and Cautions

Download spt-bms.dll then put it into your `Black Mesa\bms\` folder

`disconnect` then `plugin_load spt-bms` to load source pause tool, remember don't load it twice or your game will crash

```cfg
y_spt_autojump 1
y_spt_pause 1
```

Some settings that you must do

```cfg
fov "90"
fov_desired "90"
sv_always_run "1"
cc_subtitles "0"
closecaption "0"
cl_view_roll "0"
sv_cheats "0"
```

Optional settings

```cfg
sv_autosave "0"
snd_musicvolume "0"
cl_mdldetailfx_enable "0"
cl_pickup_colorcorrection 0

cl_auto_crouch_jump 0
cl_autoweaponpickup 0
sv_allow_color_correction 0
```

## A sample segmented cfg

segment.cfg

```cfg
y_spt_autojump 1
y_spt_pause 1


fov "90"
fov_desired "90"
sv_always_run "1"
cc_subtitles "0"
closecaption "0"
cl_view_roll "0"
sv_cheats "0"
sv_autosave "0"
snd_musicvolume "0"
cl_mdldetailfx_enable "0"
cl_pickup_colorcorrection 0

cl_auto_crouch_jump 0
cl_autoweaponpickup 0
sv_allow_color_correction 0

bind "v" "record 057-mxylr-"
bind "b" "save 057-mxylr-; sensitivity 0; echo #SAVE#; wait 100; stop"
bind "n" "load 056-rock-74; sensitivity 0"
bind "m" "unpause; sensitivity 4.0"
bind "END" "playdemo 004-mxylr-oaru-"   // it's useless because if you want to play a demo, you better use `playdemo #yourDemo#;demo_pause` then press shift+f2 to open the demo menu
```

## Voidclip script

old save load lua script by Rama

```lua
--[[
Saveload script awkward to use edition

1. Go to codepad.org, click "Lua" then paste this code and click "Submit"
2. Copy output into a cfg file
3. Load the save you want to start from and then execute cfg file
4. Wait for a million years until script stops
]]

segnum = "000" --prefix (ex 000-001, 000-002 etc)
s = 1 --seg to start from
c = 10 --number of save loads
d = 8 --load time (varies from map to map, usually in the 5-10 range)

print('_y_spt_afterframes_reset_on_server_activate 0;bind p "_y_spt_afterframes_reset_on_server_activate 1;stop"\n')

for i=s,c do
    local name = segnum.."-"..string.rep("0",3-tostring(i):len())..i
    print([[alias save]]..i..[[ "save ]]..name..[[;unpause;echo #SAVE#;wait 100;stop;wait 10;do]]..(i+1)..[["]])
    print([[alias do]]..i..[[ "reload;_y_spt_afterframes ]]..d..[[ save]]..i..[[;wait 10;record ]]..name..[["]])
end

print("alias do"..(c+1))

print("do"..s)
```

save load script re-write by _Smiley

```lua
segnum = "059-smiley" --prefix (ex 000-001, 000-002 etc)
s = 1 --seg to start from
c = 60 --number of save loads
d = 6 --load time (varies from map to map, usually in the 5-10 range)

print('bind v "y_spt_pause 1;cl_showpos 1;cl_showfps 1;r_drawviewmodel 0;cl_drawhud 0;r_drawentities 0;r_drawworld 0;-duck;wait 10;load 058-023"')
print('_y_spt_afterframes_reset_on_server_activate 0;+duck;y_spt_pause 0;bind p "_y_spt_afterframes_reset_on_server_activate 1;stop"\n')

for i=s,c do
    local name = segnum.."-"..string.rep("0",3-tostring(i):len())..i
    print([[alias save]]..i..[[ "+duck;save ]]..name..[[;unpause;echo #SAVE#;wait 100;stop;wait 10;do]]..(i+1)..[["]])
    print([[alias do]]..i..[[ "reload;+duck;_y_spt_afterframes ]]..d..[[ save]]..i..[[;wait 10;record ]]..name..[["]])
end

print("alias do"..(c+1))

print("do"..s)
```

save load script re-write by MXYLR

```python
#coding=utf-8
# author : MXYLR
# copy this code, then go to codepad.org, choose python3, then copy the outputs into a cfg file, then exec cfg file in game

segNum = "001"      # prefix (ex 000-001, 000-002 etc)
saveLoads = 50      # number of save loads
loadTime = 5        # load time (varies from map to map, usually in the 5-10 range)

print("y_spt_pause 0") # disable spt autopause to enable crouching during save loading

# bind p, that means you can press p if you want to stop the cfg script in-game
print("_y_spt_afterframes_reset_on_server_activate 0;bind p \"_y_spt_afterframes_reset_on_server_activate 1;stop\"")

for i in range(1, saveLoads + 1):
    if i < 10:
        print("alias save%d \"+duck;save %s-00%d;unpause;echo #SAVE#;wait 100;stop;wait 10;do%d\"" % (i, segNum, i, i + 1))
        print("alias do%d \"reload;+duck;_y_spt_afterframes %d save%d;wait 10;record %s-00%d\"" % (i, loadTime, i, segNum, i))
    elif i < 100:
        print("alias save%d \"+duck;save %s-0%d;unpause;echo #SAVE#;wait 100;stop;wait 10;do%d\"" % (i, segNum, i, i + 1))
        print("alias do%d \"reload;+duck;_y_spt_afterframes %d save%d;wait 10;record %s-0%d\"" % (i, loadTime, i, segNum, i))
    else:
        print("alias save%d \"+duck;save %s-%d;unpause;echo #SAVE#;wait 100;stop;wait 10;do%d\"" % (i, segNum, i, i + 1))
        print("alias do%d \"reload;+duck;_y_spt_afterframes %d save%d;wait 10;record %s-%d\"" % (i, loadTime, i, segNum, i))

print("alias do%d" % (saveLoads + 1))   # make an empty alias, to avoid the error message from the last for loop
print("do1")
```

## Other tips

### Prop clip cfg

enable this before you do propclip

```cfg
y_spt_stucksave "save #my_clip_save#; echo #SAVE#; sensitivity 0; wait 100; stop; toggleconsole"
```

then disable it after you finished the prop clip

### save delete cfg

```cfg
alias save1 "save mysave;unpause;echo #SAVE#;wait 100;stop;"
alias do1 "reload;_y_spt_afterframes 7 save1;wait 10;record #Your Current segment#"
```

then just type `save:;do1` in console

### `+use` spam cfg

```cfg
bind e "+usespam"
alias _usespam0 "+use;_y_spt_afterframes 2 -use; _y_spt_afterframes 4 _usespam"
alias +usespam "alias _usespam _usespam0; _usespam"
alias -usespam "alias _usespam"
```

### ladder boost cfg

How ladder boost works : basically jump then crouch on ladders, and you will climb ladder faster than normal

```cfg
+y_spt_duckspam;
y_spt_autojump 1
```

## Render Source demo by using SVR
