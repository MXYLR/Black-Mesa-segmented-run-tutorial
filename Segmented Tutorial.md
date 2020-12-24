# Black Mesa segmented run tutorial

## Black Mesa setup

If you know nothing about black mesa speedrun, see : https://www.speedrun.com/bms/guide/qhn4v

## What is segmented

## Segmented setup and Cautions

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

## A sample segmented run

## Voidclip script

## Other tips

### Prop clip cfg

enable this before you do propclip

```cfg
y_spt_stucksave "save #my_clip_save#; echo #SAVE#; sensitivity 0; wait 100; stop; toggleconsole"
```

then disable it after you finished the prop clip

### save delete cfg

you need change the _y_spt_afterframes number, same as the voidclip script does

```cfg
alias save1 "save :;unpause;echo #SAVE#;wait 100;stop;wait 10;do2"
alias do1 "reload;_y_spt_afterframes 10 save1;wait 10;record #Your Current segment#"
```

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
