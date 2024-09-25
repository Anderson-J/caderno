# Anotações

O gitflow serve para padronizar as branchs de desenvolvimento e em primeiro lugar não permitis commits direto para a "main", em geral é possível dividir em 3 principais categorias de branchs:

- Main
- Develop
- Feature

As features serão desenvolvidas em paralelo a branch Develop, quando for finalizada uma feature, essa deverá ser mergeada na branch Develop

Cada ponto de consolidação da branch Develop será pautada na criação de uma Release, essa release também poderá ser expressa em uma branch, essa separação pode acontecer por diversos motivos, como o fim de uma sprint por exemplo.

Já no caso de erros que são pegos em produção, uma branch de HotFix pode ser criada para esses casos em específico, é importante saber que essa branch deve ser mergeada tanto na Main quanto na Develop, uma vez que o erro deve ser corrigido para futuras Releases.

De modo generalista a  configuração de branchs fica assim:

- Main
- Develop
- Feature
- Bugfix
- Release
- Hotfix
- Support

