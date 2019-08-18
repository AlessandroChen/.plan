
<!-- vim-markdown-toc GitLab -->

* [May 2019](#may-2019)
    * [2019-05-02 21:42:45](#2019-05-02-214245)
    * [2019-05-03 20:01:01](#2019-05-03-200101)
    * [2019-05-18 18:19:34](#2019-05-18-181934)
    * [2019-05-24 21:11:14](#2019-05-24-211114)

<!-- vim-markdown-toc -->

# May 2019
## 2019-05-02 21:42:45

**conky**

I modified the i3-default conky config located at `/usr/share/conky/conky_maia`
I added some features like showing my rest storage and modified the theme.
The `conky_maia` file is now like this:
```
conky.config = {
	alignment = 'middle_right',
	background = true,
	color2 = '#dadada',
	cpu_avg_samples = 2,
	default_color = '#dadada',
	draw_shades = false,
	default_shade_color = '#2d2d2d',
	double_buffer = true,
	font = 'Monospace:size=8',
	gap_x = 10,
	gap_y = 25,
	minimum_width = 200,
	maximum_width = 200,
	no_buffers = true,
	own_window = true,
	own_window_type = 'override',
	own_window_transparent = true,
        own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
	update_interval = 2.0,
	use_xft = true,
}
conky.text = [[
${voffset 10}${goto 5}${font Dungeon:size=12}${time %a}$font${voffset -20}$alignr${color #16A085}${font Dungeon:size=45}${time %e}$font${color}
${voffset -20}${goto 5}${font Dungeon:size=12}${time %b}$font ${voffset 2}${font Dungeon:size=20}${time %Y}$font
#
${voffset 10}CPU$alignr$color2$cpu%$color
${cpubar 3,200}
$color2${top name 1}$alignr${top cpu 1}%$color
$color2${top name 2}$alignr${top cpu 2}%$color
$color2${top name 3}$alignr${top cpu 3}%$color
$color2${top name 4}$alignr${top cpu 4}%$color
$color2${top name 5}$alignr${top cpu 5}%$color
#
${voffset 10}RAM$alignr$color2$mem/$memmax$color
${membar 3,200}
$color2${top_mem name 1}$alignr${top_mem mem_res 1}$color
$color2${top_mem name 2}$alignr${top_mem mem_res 2}$color
$color2${top_mem name 3}$alignr${top_mem mem_res 3}$color
$color2${top_mem name 4}$alignr${top_mem mem_res 4}$color
$color2${top_mem name 5}$alignr${top_mem mem_res 5}$color
#
${voffset 10}Root$alignr$color2${fs_used /}/${fs_size /}$color
${fs_bar 3,200 /}
Home$alignr$color2${fs_used /home}/${fs_size /home}$color
${fs_bar 3,200 /home}
C$alignr$color2${fs_used /mnt/C}/${fs_size /mnt/C}$color
${fs_bar 3,200 /mnt/C}
D$alignr$color2${fs_used /mnt/D}/${fs_size /mnt/D}$color
${fs_bar 3,200 /mnt/D}
#
R/W: $color2${diskio_read}${goto 110}/${alignr}${diskio_write}$color
#
${voffset 10}E: Down $color2${downspeedf enp7s0}KiB${alignr}${upspeedf enp7s0}KiB$color Up
W: Down $color2${downspeedf wlp2s0}KiB${alignr}${upspeedf wlp2s0}KiB$color Up
#
${wireless_link_bar 3,200 wlp2s0}
#
#${image /usr/share/icons/hicolor/48x48/apps/manjaro-welcome.png -p 157,420}
${voffset 10}Manjaro Linux ${execi 86400 awk -F'=' '/DISTRIB_RELEASE=/ {printf $2}' /etc/lsb-release}
${kernel}-${machine}
#Host: ${nodename}
]]
```

As I found the are some messy code, I set the locale to the US
```bash
localectl set-locale LC_TIME=en_US.UTF-8
```

**i3wm**

Everytime I change my Screen Output through `xrandr`, both my wallpaper and conky will be moved.
By adding a key binding, I could use `$mod + shift + x` to do all those things at once:
```
bindsym $mod+shift+x exec "xrandr --output HDMI1 --off --output LVDS1 --off --output VIRTUAL1 --off --output DP1 --off --output VGA1 --primary --mode 1440x900 --pos 0x0 --rotate normal ; killall conky ; start_conky_maia; nitrogen --set-zoom-fill ~/Pictures/wallhaven-760730.png"
```


**mount**

Firstly, I got the UUID by `ls -all /dev/disk/by-uuid`
And add the `fat32` filesystemed disk `sda1` and `sda5` to the `/etc/fstab`, which makes me available to access `C` and `D` on my Windows
Unfortunately, `fat32` didn't support `chown` to change owner, so it's a little bit ugly when you change your directory.
```
# <file system>             <mount point>  <type>  <options>  <dump>  <pass>
UUID=d5b88742-a9a0-4696-a10a-833d626c53c7 /              ext4    defaults,noatime 0 1
UUID=3659272E1032B8F3 /mnt/C/		auto		user,umask=000,utf8 0 0
UUID=92A0B994A0B97EF3 /mnt/D/		auto		user,umask=000,utf8 0 0
```

## 2019-05-03 20:01:01

**neovim - thinkvim**

After comparing Neovim with Vim8.1, I choose to try Neovim and the wonderful configured file thinkvim.

However, I got stuck because of some requirements.

Firstly, we need to download Neovim v0.4.0 from github, since pacman only provides v0.3.4

```bash
wget https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz
tar -xzvf nvim-linux64.tar.gz
sudo mv nvim-linux64 /usr/local/
sudo ln -s /usr/local/nvim-linux64/bin/nvim /usr/bin/nvim
```

## 2019-05-18 18:19:34 

**python**

After I had upgrade the packages, python abort as follows:
```
/usr/lib/python3.7/site-packages/requests/__init__.py:91: RequestsDependencyWarning: urllib3 (1.25.2) or chardet (3.0.4) doesn't match a supported version!
```
I found this problem occured due to the forward module: urllib3, so I desided to downgrade it:
```bash
sudo pip install urllib3==1.24
```

## 2019-05-24 21:11:14

**fcitx**

Not until I typed Chinese did I realized that there is something wrong with my fcitx.

When I press `contrl+space` noting was changed, I found environment settings may be the contibutor.

After upgraded, neither `.xprofile` nor `.xinitrc` was loaded. So I press the line into `/etc/environment`:

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
export LC_CTYPE="zh_CN.UTF-8"
```

## 2019-06-22 19:57:14

When I launch `deepin-wine-qq`, Error occured as follows:

```
X Error of failed request:  BadWindow (invalid Window parameter)
  Major opcode of failed request:  20 (X_GetProperty)
  Resource id in failed request:  0x0
  Serial number of failed request:  10
  Current serial number in output stream:  10
```

To solve this, I installed the required dependency and set it to autostartup

```bash
$ sudo pacman -S cinnamon-settings-daemon
/usr/lib/cinnamon-settings-daemon/csd-xsettings
```

