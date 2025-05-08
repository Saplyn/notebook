# `.bashrc`

```bash
# Shell prompt customize
PROMPT_COMMAND=__prompt_command

__prompt_command() {
    local last_cmd_code="$?" # Need to be the first

    # Definition of colors
    local no_color='\[\e[0m\]'

    local st_bold='\[\e[1m\]'
    local st_underln='\[\e[4m\]'
    local st_reverse='\[\e[7m\]'

    local fg_black='\[\e[30m\]'
    local fg_red='\[\e[31m\]'
    local fg_green='\[\e[32m\]'
    local fg_yellow='\[\e[33m\]'
    local fg_blue='\[\e[34m\]'
    local fg_purple='\[\e[35m\]'
    local fg_cyan='\[\e[36m\]'
    local fg_white='\[\e[37m\]'

    local bg_black='\[\e[40m\]'
    local bg_red='\[\e[41m\]'
    local bg_green='\[\e[42m\]'
    local bg_yellow='\[\e[43m\]'
    local bg_blue='\[\e[44m\]'
    local bg_purple='\[\e[45m\]'
    local bg_cyan='\[\e[46m\]'
    local bg_white='\[\e[47m\]'

    # Initialize prompt
    PS1="\n${fg_blue}█${no_color}${bg_blue}${st_bold}$HOST${no_color}${fg_blue}█▓▒░${no_color}\n"
    PS2="${fg_blue}${st_bold}∙${no_color} "

    # Current working directory
    PS1+="${fg_cyan}${st_bold}\w${no_color}"
    PS1+="\n"

    # Last command status & prompt arrow
    if [ $last_cmd_code != 0 ]; then
        PS1+="${fg_red}${st_bold}❯${no_color}"
    else
        PS1+="${fg_green}${st_bold}❯${no_color}"
    fi
    PS1+=" "
}

# Quality of life aliases
alias l="ls -alF"
alias ll="ls -lF"
```
