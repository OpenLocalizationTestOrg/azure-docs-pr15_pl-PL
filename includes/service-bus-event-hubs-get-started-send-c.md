## <a name="send-messages-to-event-hubs"></a>Wysyłanie wiadomości do koncentratorów zdarzenia

W tej sekcji możemy zapisze aplikację C, aby wysyłać zdarzeń do Twoim Centrum zdarzenia. Użyjemy do biblioteki AMQP protonów [Apache Qpid projektu](http://qpid.apache.org/). To jest odpowiednikiem za pomocą usługi Bus kolejki i tematy AMQP c jak pokazano [poniżej](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Aby uzyskać więcej informacji zobacz [dokumentację protonów Qpid](http://qpid.apache.org/proton/index.html).

1. Z poziomu [programu Messenger AMQP Qpid strony](http://qpid.apache.org/components/messenger/index.html)kliknij łącze **Instalowania protonów Qpid** i postępuj zgodnie z instrukcjami w zależności od środowiska. Przyjmijmy środowiska Linux; na przykład [Maszyn wirtualnych Linux Azure](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) z Ubuntu 14.04.

2. Aby skompilować biblioteki protonów, zainstalować pakiety następujące czynności:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Pobieranie biblioteki [protonów Qpid biblioteki](http://qpid.apache.org/proton/index.html) i wyodrębnianie, np.:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Utwórz katalog kompilacji, kompilacji i instalacji:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. W katalogu pracy Utwórz nowy plik o nazwie **sender.c** o następującej treści. Pamiętaj, aby zastąpić wartość swoją nazwę Centrum zdarzeń i nazwy obszaru nazw (nie jest zazwyczaj `{event hub name}-ns`). Możesz również podstawić wersję zakodowany adres URL klucza **SendRule** utworzony wcześniej. Możesz kodowanie adresu URL go [w tym miejscu](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Opracowanie pliku, przy założeniu **Zatoce**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] W tym kodzie korzystamy wychodzących okna 1 wymusić wiadomości się, jak najwcześniej. Na ogół aplikacji należy spróbować partii wiadomości, aby zwiększyć produktywność. Wyświetlona [Strona Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html) , aby uzyskać więcej informacji na temat używania biblioteki protonów Qpid, w tym i innych środowisk i z platformy, dla których znajdują się powiązania (obecnie Perl, PHP, Python i dopiskiem).
