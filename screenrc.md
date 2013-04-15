```shell
#escape "^Tt"

#screen -t Shell 0 
#screen -t Emacs 1 emacs -nw
#screen -t Bash  2 bash
#select 0
#kill
#select 1
vbell off
autodetach on
defscrollback 5000
#allpartial on
#partial on

#termcapinfo xterm ti@:te@

shelltitle "$|bash"

#caption always "%{wb} %-w%{+b bw}%n %t%{-}%+w"

hardstatus on
hardstatus alwayslastline
# hardstatus string "%{.kW}%-w%{.rW}%n %t%{-}%+w %=%{..G} %H %{..Y} %m/%d %C%a "
# hardstatus string '%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %m/%d/%Y %{W}%c:%s %{g}]'
hardstatus string '%{= kG} %{G}%H %{g}[%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}]%{B} %{W}%c %{g}'

bind g
bind ^g
bind L

#term screen-256color
term xterm
# attrcolor b ".I"
# bce on

altscreen on

defwrap on
defflow off # for emacs C-x C-s
# encoding GBK GBK
encoding UTF8 UTF8

shell -$SHELL
```
