 s
Esta lista es la oficial de bash bunni:

https://www.youtube.com/watch?v=ZvhzrltuA9U&list=PL3PYGQRVAjrMxP5HK45CTnR7Yv-QYR1Qp&index=2

el primer video no me funciono del todo pues al tratar de ejecutar PlugInstall me daba un error, la clave esta en el archivo ~/.config/nvim/init.vim

con la ayuda de Copilot:

Certainly! It seems like you’re encountering an issue with **vim-plug** in NeoVim. Let’s troubleshoot this together.

1. First, ensure that you have **installed git**. If not, you can install it using the following command:
    
    ```
    sudo apt-get install git
    ```
    
2. Next, let’s verify your NeoVim configuration file. You mentioned that the output of `echo $MYVIMRC` is “/home/hftamayo/.config/nvim/init.lua”. However, NeoVim expects the configuration file to be named **“init.vim”** instead of “init.lua”. Please rename your configuration file to “init.vim” and make sure it resides in the correct location: `~/.config/nvim/init.vim`.
    
3. After renaming the file, open NeoVim and try running `:PlugInstall` again. It should work as expected.
    
4. Additionally, ensure that you have correctly set up your **vim-plug** configuration. You can follow the steps in the to ensure everything is in place. (https://github.com/junegunn/vim-plug/wiki/tutorial)
    

Remember to **source your configuration file** (`:source ~/.config/nvim/init.vim`) after making any changes. If you encounter any further issues, feel free to ask for more assistance! 🚀

Si sigo el proceso con este commit todo el proceso es mas facil: 

Certainly! To clone a specific commit from a Git repository, follow these steps:

1. **Clone the main repository**:
    
    - Run the following command to clone the repository:
        
        ```
        git clone https://github.com/bashbunni/dotfiles.git
        ```
        
    - Navigate into the cloned repository:
        
        ```
        cd dotfiles
        ```
        
2. **Reset to the desired commit**:
    
    - Identify the commit hash you want to clone (in your case, it’s `38bd2e94703c39854257649701e46fe95ea7844e`).
    - Use the following command to reset your working directory to that specific commit:
        
        ```
        git reset --hard 38bd2e94703c39854257649701e46fe95ea7844e
        ```
        
3. **Clean untracked files (optional)**:
    
    - If you want to remove any untracked files or directories, run:
        
        ```
        git clean -df
        ```
        

Now you have a local copy of the repository at the specific commit you specified. Feel free to make any further changes or work with the code as needed! 🚀

![[Pasted image 20231222155247.png]]

la idea es instalar todos estos plugins

hay un par de cosas como la de migrar a nvim 0.8 y otros aspectos pero en general se ve bastante bien.