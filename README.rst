aiotg
=====

Telegram botları oluşturmak için asenkron Python API'si şunları içerir:

- Kolay ve bildirimsel API
- Sorunsuz kurulum - SSL sertifikalarına veya statik IP'ye gerek yok
- Chatbase.com üzerinden analitik için yerleşik destek
- Telegram API daraltma veya zaman aşımlarının otomatik olarak işlenmesi

pip ile kurun:

.. code:: sh

    pip install aiotg

Ardından birkaç satırda yeni bir bot oluşturabilirsiniz.:

.. code:: python

    from aiotg import Bot, Chat

    bot = Bot(api_token="...")

    @bot.command(r"/echo (.+)")
    def echo(chat: Chat, match):
        return chat.reply(match.group(1))

    bot.run()


Şimdi uygun bir API\_TOKEN ile çalıştırın ve /echo komutlarına yanıt vermelidir.

.. note:: Type annotations are not required but will help your editor/IDE to provide code completion.

Yukarıdaki örnek normal bir senkron koda benziyor ama aslında bir eşyordam döndürüyor.
Harici bir istekte bulunmak istiyorsanız (ve botların genellikle yaptığı şey budur) sadece aiohttp ve async/await s kullanın.

.. code:: python

    import aiohttp
    from aiotg import Bot, Chat

    bot = Bot(api_token="...")

    @bot.command("bitcoin")
    async def bitcoin(chat: Chat, match):
        url = "https://apiv2.bitcoinaverage.com/indices/global/ticker/BTCUSD"
        async with aiohttp.ClientSession() as session:
            response = await session.get(url)
            info = await response.json()
            await chat.send_text(str(info["averages"]["day"]))

    bot.run()


Peki ya sadece hızlı bir entegrasyon yazmak istiyorsanız ve bir konuşma arayüzü sağlamanız gerekmiyorsa? Seni koruduk!
Bir Chat ob oluşturarak mesajlar (veya başka herhangi bir medya) gönderebilirsiniz.

.. code:: python

    ...
    channel = bot.channel("@yourchannel")
    private = bot.private("1111111")

    async def greeter():
        await channel.send_text("Hello from channel!")
        await private.send_text("Why not greet personally?")
    ...


Örnekler
---------------

- `Async IO <https://github.com/frozenbey/aiotg/blob/master/examples/async.py>`__
- `Send image <https://github.com/frozenbey/aiotg/blob/master/examples/getimage.py>`__
- `Post to channel <https://github.com/frozenbey/aiotg/blob/master/examples/post_to_channel.py>`__
- `Webhooks mode <https://github.com/frozenbey/aiotg/blob/master/examples/webhook.py>`__
- `Sender id <https://github.com/frozenbey/aiotg/blob/master/examples/whoami.py>`__
- `WhatisBot <https://github.com/frozenbey/whatisbot/blob/master/main.py>`__ or `Music Catalog Bot <https://github.com/szastupov/musicbot>`__.

Projenin nasıl kullanılacağı hakkında daha fazla bilgi için projenin `belgelerine <http://szastupov.github.io/aiotg/index.html>`__ bakın.

