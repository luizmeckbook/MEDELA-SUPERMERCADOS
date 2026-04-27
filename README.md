import com.sun.net.httpserver.HttpServer;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpExchange;
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;

public class App {
    public static void main(String[] args) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);

        server.createContext("/", new HomeHandler());
        server.createContext("/acao", new AcaoHandler()); // rota do botão

        server.setExecutor(null);
        System.out.println("Servidor rodando em http://localhost:8080");
        server.start();
    }

    // Página principal
    static class HomeHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange exchange) throws IOException {

            String response = """
                <html>
                <head>
                    <meta charset="UTF-8">
                    <style>
                        body {
                            font-family: Arial;
                            text-align: center;
                            background: #f5f5f5;
                        }
                        h1 { color: green; }

                        .box {
                            background: white;
                            padding: 20px;
                            margin: 20px auto;
                            width: 300px;
                            border-radius: 10px;
                            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
                        }

                        button {
                            padding: 10px 20px;
                            border: none;
                            background: green;
                            color: white;
                            border-radius: 5px;
                            cursor: pointer;
                        }

                        button:hover {
                            background: darkgreen;
                        }
                    </style>
                </head>
                <body>

                    <h1>RUAN ASSISTEC</h1>

                    <div class="box">
                        <p>Clique no botão:</p>

                        <form action="/acao" method="get">
                            <button type="submit">Clique aqui</button>
                        </form>

                    </div>

                </body>
                </html>
            """;

            exchange.getResponseHeaders().set("Content-Type", "text/html; charset=UTF-8");
            exchange.sendResponseHeaders(200, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }

    // Ação do botão
    static class AcaoHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange exchange) throws IOException {

            String response = """
                <html>
                <body style="text-align:center; font-family:Arial;">
                    <h1>Botão Funcionou ✅</h1>
                    <a href="/">Voltar</a>
                </body>
                </html>
            """;

            exchange.getResponseHeaders().set("Content-Type", "text/html; charset=UTF-8");
            exchange.sendResponseHeaders(200, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}
