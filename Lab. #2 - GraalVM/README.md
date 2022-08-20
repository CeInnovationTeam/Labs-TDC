## Desafio 02 - GraalVM
- [Criando VCN e Compute Instace (Oracle Linux 8)](/README.md#passo-1---crie-a-virtual-cloud-network-vcn-e-uma-compute-instance-com-a-imagem-pré-criada-do-oracle-linux-8-)

- [Instalando o GraalVM Enterprise Oracle Linux](/README.md#passo--2-instale-o-graalvm-enterprise-oracle-linux)

- [Inserindo recursos adicionais do GraalVM Enterprise](/README.md#passo-3----adicione-recursos-adicionais-do-graalvm-enterprise-imagem-nativa)

- [Atualizando a instalação do GraalVM Enterprise](/README.md#passo-4-atualizar-uma-instalação-existente-do-graalvm-enterprise)





### PASSO 1 - crie a Virtual Cloud Network (VCN) e uma Compute Instance com a imagem pré-criada do Oracle Linux 8. 💻

1. Acesse o menu OCI > Rede; 

2. Clique em Iniciar Assistente de VCN;

3. Selecione Criar VCN com Conectividade com a Internet;

4. Clique em Iniciar Assistente de VCN novamente;

5. Preencha com um nome;

6. Clique em Review and Create em seguida, clique em Create e aguarde;

7. Acesse o menu OCI > Compute > Instâncias;

8. Clique em Criar Instância;

9. Alterar ou nomear e validar Imagem = Oracle Linux 8;

10. Em adicionar chaves SSH, selecione Gerar um par de chaves para mim;

11. Faça o download da chave pública e privada;

12. Clique em criar e aguardar ou provisionar; 

13. Copie o Pubci IP address da instância e salve em lugar seguro;



### PASSO  2: Instale o GraalVM Enterprise Oracle Linux 🟢
Nesta tarefa, você instalará o GraalVM Enterprise no Oracle Linux e o definirá como um tempo de execução Java padrão.

1. Para uma instalação conveniente, os RPMs do GraalVM Enterprise estão disponíveis no repositório Oracle Linux YUM, o que significa que os usuários do OCI podem instalar o GraalVM Enterprise em suas instâncias de nuvem usando o yum — um utilitário de gerenciamento de pacotes para os sistemas operacionais Linux.

2. (Opcional) Na janela do terminal conectada a uma instância de VM, pesquise os pacotes GraalVM Enterprise disponíveis, restringindo a pesquisa a uma versão específica e Java 11:
    - *sudo yum fornece graalvm21-ee-11-jdk*
    - A lista resultante inclui as versões atuais e anteriores do Oracle GraalVM Enterprise Edition JDK11 Java Development Kit versão 21.x.

3. Instale graalvm21-ee-11-jdk:
    - *sudo yum install graalvm21-ee-11-jdk*

4. Confirme se o tamanho do pacote instalado está correto digitando yes no prompt. Ele instalará a versão mais recente do graalvm21-ee-11-jdk, que inclui o tempo de execução da JVM, o compilador Graal e todos os pacotes dependentes, por exemplo, libpolyglot, llvm, etc.

5. Configure as variáveis de ambiente para apontar para a instalação do GraalVM Enterprise para esta sessão SSH. Após a instalação, os arquivos do pacote são colocados no diretório /usr/lib64/graalvm e os binários em bin de acordo.

6. Defina as variáveis de ambiente PATH e JAVA_HOME na configuração do bash para apontar para o GraalVM Enterprise com os seguintes comandos:
    - *echo "export JAVA_HOME=/usr/lib64/graalvm/graalvm21-ee-11" >> ~/.bashrc*
    - *echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc*
    - Activate this change: *source ~/.bashrc*

7. Verifique a versão do Java para ver se a instalação foi bem-sucedida e o JDK está configurado para GraalVM Enterprise:
    - *java -version*

8. Neste ponto, você já pode executar qualquer carga de trabalho Java no GraalVM Enterprise sem a necessidade de alterações de código. O GraalVM Enterprise emprega o compilador de otimização Graal como o compilador JIT de primeira linha que realiza otimização avançada e aplica técnicas agressivas de inlining para acelerar o desempenho do aplicativo.

9. Recomendamos que você faça o laboratório Accelerate Applications in Oracle Cloud with GraalVM Enterprise após a conclusão deste laboratório. Esse laboratório se concentra em comparar o desempenho do compilador Graal JIT vs. C2 durante a execução do Java Microbenchmark Harness (JMH).

10. Você pode prosseguir para a próxima tarefa.



### PASSO 3 -  adicione recursos adicionais do GraalVM Enterprise (imagem nativa) ➕

1. O GraalVM Enterprise é fornecido com componentes principais (para salvar o tamanho do arquivo) e pode ser estendido com mais recursos sob demanda. Por exemplo, você pode instalar a imagem nativa, o runtime do Node.js, a cadeia de ferramentas LLVM etc. Verifique a documentação do produto para obter mais informações sobre os recursos disponíveis.

2. Para adicionar recursos adicionais ao GraalVM Enterprise, o comando yum install <package_name> é suficiente. Nesta tarefa, você instalará o Native Image do GraalVM Enterprise, uma tecnologia para compilar antecipadamente o código Java para um executável nativo autônomo.

3. (Opcional) Verifique quais recursos adicionais estão disponíveis para sua instalação atual do GraalVM Enterprise:
    - *sudo yum provides graalvm21**


4. A lista é enorme. Como você está interessado no componente Native Image, restrinja a pesquisa fornecendo o nome exato do pacote:

    - *sudo yum fornece graalvm21-ee-11-native-image**


5. Instale o Native Image executando estes comandos um por um (específico para Oracle Linux 8):
    - *sudo yum update -y oraclelinux-release-el8*


6. Ele atualizará o cache de metadados do repositório local para obter novos pacotes disponíveis.
    - *sudo yum config-manager --set-enabled ol8_codeready_builder*

7. Ele habilitará o repositório ol8_codeready_builder contendo algumas das dependências de imagens nativas.
    - *sudo yum install graalvm21-ee-11-native-image*

8. Confirme se o tamanho do pacote instalado está correto digitando yes no prompt. Ele instalará todas as bibliotecas dependentes necessárias (por exemplo, glibc, zlib, etc.) e colocará o utilitário de imagem nativa no diretório de instalação do GraalVM Enterprise ($JAVA_HOME/bin).

9. Verifique a versão para ver se a instalação foi bem-sucedida:
    - *native-image --version*

10. Agora você pode começar a usar o utilitário native-image para transformar seu aplicativo Java em um executável Linux nativo. A execução de um aplicativo Java como um executável nativo fornece inicialização instantânea, menor consumo de CPU e memória, tornando-o um bom candidato para implantações em nuvem.

### PASSO 4: atualizar uma instalação existente do GraalVM Enterprise 🆙

O gerenciador de pacotes yum para Oracle Linux pode ser usado para atualizar uma instalação existente do GraalVM Enterprise ou substituí-la por outra versão. Nesta tarefa, você atualizará o GraalVM Enterprise da versão 21.x para 22.x e substituirá a distribuição do Java 11 pelo GraalVM Enterprise for Java 17.

1. Atualize o GraalVM Enterprise da versão 21.x para 22.xe instale a distribuição para Java 17 em vez de Java 11:
    - *sudo yum install graalvm22-ee-17-jdk*

2. Confirme se o tamanho do pacote instalado está correto digitando yes no prompt.

3. Verifique a versão do Java para ver se a atualização foi bem-sucedida:
    - *java -version*

4. O pacote graalvm22-ee-17-jdk foi instalado junto com graalvm21-ee-11-jdk no diretório /usr/lib64/graalvm e todo o sistema foi atualizado. 
![](link.img)
    - (Nota: Independentemente da versão impressa no console, as variáveis de ambiente PATH e JAVA_HOME ainda apontam para a versão antiga) Redefina as variáveis conforme  abaixo:

    - *echo "export JAVA_HOME=/usr/lib64/graalvm/graalvm21-ee-17" >> ~/.bashrc*

    - *echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc*

    - *source ~/.bashrc*

    - java -version
    
    
5. O comando yum upgrade pode ser usado para atualizar na linha do pacote do mesmo ano, por exemplo, para atualizar do GraalVM Enterprise 22.0.0 para a versão 22.0.1 quando este pacote RPM estiver disponível:
    - *sudo yum upgrade graalvm22-ee-17-jdk*

6. le atualizará todo o sistema e removerá a instalação obsoleta do GraalVM Enterprise.

## 🎊🏆Parabéns! Você concluiu este laboratório com sucesso 🏆🎊
