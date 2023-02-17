# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.zshrc.
# Initialization code that may require console input (password prompts, [y/n]
# confirmations, etc.) must go above this block; everything else may go below.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH
export PYENV_ROOT=$HOME/.pyenv
export PATH=${PATH}:/home/dm/.local/bin:$PYENV_ROOT/bin

# Path to your oh-my-zsh installation.
export ZSH="/home/dm/.oh-my-zsh"
export DD_APIv2_TOKEN="21a5dbebcf616756c076157f6e6dc1f3d6b4967e"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes

########################################################################################
#   Comment for disable error see https://github.com/romkatv/powerlevel10k/issues/622  #
########################################################################################
# ZSH_THEME="powerlevel10k/powerlevel10k"

POWERLEVEL9K_MODE="nerdfont-complete"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to automatically update without prompting.
# DISABLE_UPDATE_PROMPT="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# Caution: this setting can cause issues with multiline prompts (zsh 5.7.1 and newer seem to work)
# See https://github.com/ohmyzsh/ohmyzsh/issues/5765
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git 
		 zsh-syntax-highlighting 
		 zsh-autosuggestions 
		 colorize
		 docker-compose 
		 cargo 
		 npm 
		 pip 
		 dirhistory 
		 autojump
		 web-search 
		 alias-finder)

source $ZSH/oh-my-zsh.sh
source /usr/share/nvm/init-nvm.sh 

########################################################################################
# Config for colorize https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/colorize##
#######################################################################################

# ZSH_COLORIZE_TOOL=chroma
ZSH_COLORIZE_CHROMA_FORMATTER="true-color"
ZSH_COLORIZE_STYLE="zenburn"

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

alias scp='scp -r'
alias rm='rm -r'
alias mkdir='mkdir -p'

alias ls='ls --color=auto'
alias la='ls -A --color=auto'
alias ll='ls -lh --color=auto -h'
alias lla='ll -Ah --color=auto -h'
alias cp='cp -r'

alias grep='grep --colour=auto'

alias openfortigui='/usr/bin/openfortigui > /dev/null 2>&1 &'

# servers ssh

alias ssh='TERM=xterm-256color ssh'

alias vps-ssh='ssh zakrush@45.137.64.132 -p 2210 -i ~/.ssh/zomro'
alias fuzz-server='ssh user@fuzzing01.sec.dev.rvision.local'
alias msfconsole="msfconsole -x \"db_connect user@metasploit\""
alias defect="ssh user@defect-dojo.sec.dev.rvision.local"
alias zapserver="ssh user@zap01.sec.dev.rvision.local"
alias zomro="ssh zakrush@45.137.64.132 -p 2210"

alias zaproxy='zaproxy > /dev/null 2>&1 &'
alias dc="docker-compose"

#tools
alias sqlmap="python ~/Job/tools/sqlmap/sqlmap.py"




alias google="web_search google" 

alias cat="ccat"
alias jsfuzz='node /home/dm/Job/R-Vision/tools/jsfuzz/build/src/index.js'



#alias cat='ccat'


source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

# Enable autojump
[[ -s /home/dm/.autojump/etc/profile.d/autojump.sh ]] && source /home/dm/.autojump/etc/profile.d/autojump.sh

[[ -d $HOME/go/bin ]] && export PATH=$HOME/go/bin:$PATH

# autoload -U compinit && compinit -u
