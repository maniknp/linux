```bash
sudo apt install tmux
```

```bash
# start tmux 
tmux    
```
**How to Exit from `tmux` terminal**
ctrl + b  then press `d` button (Key)  


```bash
# attach previous session 
tumx a 
```

### Create a session and name it 
*Syntax:* tmux new -s <name of session >  
```bash  
tmux new -s bob
```

### list all session 
```bash
tmux ls
```

### Attach specific session 
```bash
tmux a -t 0
```
Note:
 - a = attach 
 - -t = target 
 - 0 = is index number ( you can select num from `tmux ls`)

### kill a session 
```bash
tmux kill-session -t bob
```

### Split screen

**Vertical**: 
ctrl + b  then `%`

**Horizontal split**
ctrl +b then `"`

**Check screen Indexes**

ctrl +b then `q`    ==> It  will show all screen index
ctrl +b then `q` <index number of screen>   ==> you will switch to particular screen 
ctrl +b then  `direction arrow key`  ==> switch to a screen ( it once a screen every `ctrl+ b` command ) 

ctrl +b then alt +  `direction arrow key`    ==> adjust the screen size 

### Creating window
ctrl +b then `c`    ==> It  will create a window
ctrl +b then `n`    ==> It will just move you squentially through window
ctrl +b then `,`    ==> rename the current window
ctrl +b then `w`    ==>  it will list all windows ( Here i can navigate therough the arrow keys)

**Killing Pane**
ctrl +b then `x`    ==> It  will kill a pane
**Killing window**
ctrl +b then `&`    ==> It  will kill a window 

**Kill every thing once **
tmux kill-server

