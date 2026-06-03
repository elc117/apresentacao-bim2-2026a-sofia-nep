# Apresentação Sofia Etchepare Daronco

## Parte 1

O programa exibe uma *race condition*, que pode ser corrigida através do uso de *synchronized* nos métodos que modificam a conta. Assim, o mesmo método não pode ser chamado duas vezes ao mesmo tempo em um objeto.  
Original:

TBD

Corrigido:

```java
class Conta {

  private float saldo = 0f;

  public Conta(float inicial) {
    saldo = inicial;
  }

  public float getSaldo() {
    return saldo;
  }

  public synchronized void deposita(float valor) {
    saldo += valor;
  }

  public synchronized void retira(float valor) {
    saldo -= valor;
  }
}
```

TBD

## Parte 2

Utilizei um dos meus projetos, https://git.bottomservices.club/nep/DiscordRPC4j16.
O método em https://git.bottomservices.club/nep/DiscordRPC4j16/src/commit/68d21141ab1f3c41f2297b869717d3db5a8fd03f/src/main/java/club/bottomservices/discordrpc/lib/Utils.java#L49 é *synchronized*, isso significa que threads diferentes não podem executar esse método ao mesmo tempo. Isso é necessário pois esse método utiliza um boolean para garantir que só é chamado uma vez no programa inteiro, mas ser chamado por threads diferentes rapidamente permitiria que a execução ocorresse mais de uma vez (antes do boolean se tornar *true*).

```java
public static synchronized void loadNative() throws IOException {
    if (loaded) {
        return;
    }

    try (InputStream lib = Utils.class.getResourceAsStream("/libdiscordrpc.so")) {
        if (lib == null) {
            throw new NoDiscordException("Java < 16 and no fallback native");
        }

        File tmp = File.createTempFile("libdiscordrpc", ".so");
        tmp.deleteOnExit();
        try (OutputStream out = Files.newOutputStream(tmp.toPath())) {
            byte[] libBytes = new byte[lib.available()];
            while (lib.read(libBytes) > 0) {
                libBytes = Arrays.copyOf(libBytes, libBytes.length + lib.available());
            }
            out.write(libBytes);
        }
        System.load(tmp.getCanonicalPath());
        loaded = true;
    }
}
```