Como parte da avaliação, forneça seu entendimento sobre cada camada do projeto:

1. Qual o papel da camada Domain?
   Representa as regras de negócio e as entidades do sistema, independentemente de tecnologias externas. Contém lógicas e regras que descrevem o problema do domínio.

2. Qual o papel da camada Infrastructure?
   Responsável pela interação com sistemas externos, como banco de dados, APIs ou fila de mensagens. Ela fornece implementações concretas para armazenar ou recuperar dados.

3. Qual o papel da camada WebApp?
   Lida com a interação do usuário, seja por interface web ou APIs. Exibe informações, recebe dados e consome serviços da camada de backend.

4. Aponte um ponto de melhoria que considere relevante no projeto.
   Melhorar a abstração das dependências externas na camada Infrastructure, usando interfaces que permitem a troca de tecnologias sem impactar o resto do sistema. Isso facilita a manutenção e escalabilidade do projeto.
