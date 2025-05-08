# `.tmux.conf`

```bash
#!/usr/bin/env bash

# Enable color modes
set -g default-terminal "screen-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"

# Disable key escape
set -sg escape-time 0

# Quality of life
set -g mouse on
setw -g mode-keys vi
setw -g history-limit 10000000
set-option -g focus-events on

# Clipboard
set-option -s set-clipboard off
bind -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "xclip -i -f -selection primary | xclip -i -selection clipboard"

# Quick resize
bind-key -r -T prefix C-h resize-pane -L 2
bind-key -r -T prefix C-j resize-pane -D 2
bind-key -r -T prefix C-k resize-pane -U 2
bind-key -r -T prefix C-l resize-pane -R 2

# Easy reload config file
bind r source-file ~/.tmux.conf

# Move around panes with hjkl
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Splite Window
bind v split-window -h
bind s split-window -v

########## Styling ##########

# Pane border color
set-option -g pane-active-border-style fg=colour213
set-option -g pane-border-style fg=colour239

# Pane number display
set-option -g display-panes-active-colour colour201
set-option -g display-panes-colour colour189

# Clock
set-window-option -g clock-mode-colour colour207

# Enable status bar
set-option -g status "on"
set-option -g monitor-bell "on"
set-option -g monitor-activity "on"

# Status bar basic setting
set -g status-interval 1
set -g status-left-length 40
set -g status-right-length 30

# [win/cur{/}tab/----------/time]
set-window-option -g window-status-separator ""

# [win/cur/tab/{----------}/time]
set-option -g status-style bg=colour0,fg=colour255

# [{win}/cur/tab/----------/time]
set-option -g status-left "\
#[bg=#5e5faf, fg=colour189]#{?client_prefix,#[bg=colour167],}  #{?window_zoomed_flag, 󰻿 ,}#S \
#[bg=colour0, fg=#5e5faf]#{?client_prefix,#[fg=colour167],}"

# [win/{cur}/tab/----------/time]
set-window-option -g window-status-current-format "\
#[bg=colour0, fg=#383969] \
#[bg=#5e5faf, fg=colour189] #I \
#[bg=colour189, fg=#5e5faf] \
#[bg=colour189, fg=#5e5faf, bold]#W "

# [win/cur/{tab}/----------/time]
set-window-option -g window-status-format "\
#[bg=colour0, fg=#383969] \
#[bg=#383969, fg=colour189] #I \
#[bg=#383969, fg=colour189]#{?window_activity_flag,󱥂  ,}\
#[bg=colour189, fg=#383969]#{?window_bell_flag, 󱅫 ,}\
#[bg=#414349, fg=colour189] #W "

# [win/cur/{bell}/----------/time]
set-window-option -g window-status-bell-style bg=#414349,fg=colour189

# [win/cur/tab/----------/{time}]
set-option -g status-right "\
#[bg=#5e5faf, fg=colour189] %d %b %Y %H:%M "

# Window list
set-window-option -g mode-style bg=#5e5faf,fg=colour189

# Message, Activity
set-option -ag message-style bg=colour167,fg=colour189
set-option -ag window-status-activity-style none
```
