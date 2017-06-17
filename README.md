## Introdução
Este Vagrantfile é capaz de criar uma máquina virtual configurada para acessar sites (`Banco do Brasil`, `Caixa Econômica Federal`, etc) que exigem o módulo de segurança [WARSAW](http://www.dieboldnixdorf.com.br/gas-antifraude). Atualmente somente o internet banking da [CEF](https://internetbanking.caixa.gov.br/
) e [BB](https://www2.bancobrasil.com.br/aapf/login.jsp) foram testados neste projeto.

## Pré-requisitos
- Vagrant 1.8.4
- VirtualBox 5.1.20

## Utilização
Após `vagrant up`, aguarde a abertura do Firefox no Ubuntu (o site da CEF será aberta por padrão). Isso pode levar vários minutos pois diversos pacotes serão baixados e configurados.

## Agradecimentos
[Fábio Ribeiro](http://www.farribeiro.com.br) e [Laércio de Sousa](https://disqus.com/by/LaercioSousa/) por disponibilizar [wscef-docker](https://github.com/farribeiro/wscef-docker).

[wscef-docker](https://github.com/farribeiro/wscef-docker) trata-se de um container docker capaz de executar o [WARSAW](http://www.dieboldnixdorf.com.br/gas-antifraude).
