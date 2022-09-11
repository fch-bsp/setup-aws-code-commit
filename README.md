# Instrução de adicionar no AWS Code Commit
Você pode usar o Console de gerenciamento da AWS e fazer upload, adicionar ou editar um arquivo em um repositório diretamente do console do AWS CodeCommit. Esta é uma maneira rápida de fazer uma mudança. No entanto, se você quiser trabalhar com vários arquivos, arquivos entre ramificações e assim por diante, considere configurar seu computador local para trabalhar com repositórios. Nesta demonstração, aprenderemos como configurar o AWS Code Commit usando funções SSH e IAM.


![Set Up SSH Connections to AWS CodeCommit Repositories](https://raw.githubusercontent.com/miztiik/setup-aws-code-commit/master/images/SSH%20Connections%20to%20AWS%20CodeCommit%20Repositories.png)


## Set Up SSH Connections to AWS CodeCommit Repositories

_**Note:** This is for Linux/Mac users._

1. ### Create IAM Users/Groups
    It is better to have a seperate groups(say `Devs`) and add your users to that group.

    Add Group Permission - Managed Policy - `AWSCodeCommitFullAccess`

1. ### Create SSH Keys
    ```sh
    # Create the `.ssh` directory if it isn't there already
    # mkdir -p $HOME/.ssh
    cd $HOME/.ssh
    ssh-keygen
    # [here just create the name codecommit_rsa and leave all fields blank *just click enter*]
    cat codecommit_rsa.pub  
    ```
1. ### Associate Your Public Key with Your IAM User
    - Now we need to enter our `codecommit_rsa.pub` into AWS IAM. 
    - Copy the SSH key ID (for example, `APKAEIBAERJR2EXAMPLE`)
1. ### Add AWS CodeCommit to Your SSH Configuration
    ```sh
    cd $HOME/.ssh
    touch config
    chmod 600 config
    cat > $HOME/.ssh/config << "EOF"
    Host git-codecommit.*.amazonaws.com
      User YOUR_SSH_KEY_ID_FROM_IAM
      IdentityFile ~/.ssh/codecommit_rsa
    EOF
    ```

1. ### Test your SSH configuration:
    ```sh
    ssh git-codecommit.us-east-1.amazonaws.com
    ```

You should see something like this,
> You have successfully authenticated over SSH. You can use Git to interact with AWS CodeCommit. Interactive shells are not supported.Connection to git-codecommit.us-east-1.amazonaws.com closed by remote host.
