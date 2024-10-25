import com.google.gson.Gson;
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;

public class CrudCrudClient {

    private static final String API_URL = "https://crudcrud.com/api/TUO_ENDPOINT/items"; // Cambia TUO_ENDPOINT

    private static String sendRequest(String url, String method, String jsonInput) throws IOException {
        URL obj = new URL(url);
        HttpURLConnection con = (HttpURLConnection) obj.openConnection();
        con.setRequestMethod(method);
        con.setRequestProperty("Content-Type", "application/json");

        if (jsonInput != null) {
            con.setDoOutput(true);
            try (OutputStream os = con.getOutputStream()) {
                byte[] input = jsonInput.getBytes("utf-8");
                os.write(input, 0, input.length);
            }
        }

        int responseCode = con.getResponseCode();
        System.out.println("Response Code: " + responseCode);

        try (BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()))) {
            String inputLine;
            StringBuilder response = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            return response.toString();
        }
    }

    // Metodo POST
    public static void createItem(Item item) throws IOException {
        Gson gson = new Gson();
        String jsonInput = gson.toJson(item);
        String result = sendRequest(API_URL, "POST", jsonInput);
        System.out.println("POST Response: " + result);
    }

    // Metodo GET
    public static void getItems() throws IOException {
        String result = sendRequest(API_URL, "GET", null);
        System.out.println("GET Response: " + result);
    }

    // Metodo DELETE
    public static void deleteItem(String id) throws IOException {
        String result = sendRequest(API_URL + "/" + id, "DELETE", null);
        System.out.println("DELETE Response: " + result);
    }

    // Metodo PUT
    public static void updateItem(String id, Item item) throws IOException {
        Gson gson = new Gson();
        String jsonInput = gson.toJson(item);
        String result = sendRequest(API_URL + "/" + id, "PUT", jsonInput);
        System.out.println("PUT Response: " + result);
    }
}
