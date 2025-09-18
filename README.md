import com.opencsv.CSVWriter;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.FileWriter;
import java.io.IOException;

public class WebScraper {
    public static void main(String[] args) {
        String url = "https://books.toscrape.com/";
        String csvFile = "products.csv";

        try (CSVWriter writer = new CSVWriter(new FileWriter(csvFile))) {
          
            String[] header = {"Product Name", "Price", "Rating"};
            writer.writeNext(header);

            Document doc = Jsoup.connect(url).get();

            Elements products = doc.select(".product_pod");

            for (Element product : products) {
                String name = product.select("h3 a").attr("title");
                String price = product.select(".price_color").text();
                String rating = product.select(".star-rating").attr("class")
                        .replace("star-rating", "").trim();

                
                String[] row = {name, price, rating};
                writer.writeNext(row);

                System.out.println("Scraped: " + name + " | " + price + " | " + rating);
            }

            System.out.println("Data saved to " + csvFile);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
# E-commerce-Scraper
