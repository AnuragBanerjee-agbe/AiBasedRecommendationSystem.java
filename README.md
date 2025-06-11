import org.apache.mahout.cf.taste.impl.model.file.FileDataModel; import org.apache.mahout.cf.taste.impl.recommender.svd.ALSWRFactorizer; import org.apache.mahout.cf.taste.impl.recommender.svd.SVDRecommender; import org.apache.mahout.cf.taste.model.DataModel; import org.apache.mahout.cf.taste.recommender.RecommendedItem; import org.apache.mahout.cf.taste.recommender.Recommender;

import java.io.File; import java.util.List; import java.util.Scanner;

public class AIProductRecommender {

public static void main(String[] args) {
Scanner scanner = new Scanner(System.in);

try {  
    // LOAD USER-ITEM RATING DATA FROM CSV FILE  
    File dataFile = new File("data.csv");  
    DataModel model = new FileDataModel(dataFile);  

    // CREATE RECOMMENDER USING MATRIX FACTORIZATION  
    ALSWRFactorizer factorizer = new ALSWRFactorizer(model, 15, 0.05, 20);  
    Recommender recommender = new SVDRecommender(model, factorizer);  

    // ASK FOR USER ID FROM CONSOLE  
    System.out.print("PLEASE ENTER YOUR USER ID: ");  
    long userId = scanner.nextLong();  

    // NUMBER OF RECOMMENDATIONS TO GENERATE  
    int numberOfRecommendations = 5;  

    // FETCH AND DISPLAY RECOMMENDATIONS  
    List<RecommendedItem> recommendations = recommender.recommend(userId, numberOfRecommendations);  

    if (recommendations.isEmpty()) {  
        System.out.println("SORRY! NO RECOMMENDATIONS FOUND FOR USER ID: " + userId);  
    } else {  
        System.out.println("\nPERSONALIZED PRODUCT RECOMMENDATIONS:");  
        for (RecommendedItem item : recommendations) {  
            System.out.printf("PRODUCT ID: %-5s | SCORE: %.2f\n", item.getItemID(), item.getValue());  
        }  
    }  

} catch (Exception e) {  
    System.out.println("AN ERROR OCCURRED WHILE PROCESSING RECOMMENDATIONS:");  
    e.printStackTrace();  
} finally {  
    scanner.close();  
}

}

}
