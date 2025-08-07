import java.util.Scanner;

public class RailFenceCipher {
    public static String encrypt(int rails, String text){
        if(rails<=1){
            return text;
        }

        char[][] railMatrix = new char[rails][text.length()];

        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                railMatrix[i][j] ='\0';  
            }
        }

        boolean down = false;
        int row = 0;

        for(int i=0;i<text.length();i++){
            if(row == 0 || row == rails-1){
                down = !down;
            }

            railMatrix[row][i] = text.charAt(i);
            row += down ? 1 : -1;
        }

        System.out.println();
        System.out.println("Printing the matrix for encryption............");
        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                System.out.print(railMatrix[i][j] != '\n' ? railMatrix[i][j] + " " : " ");
            }
            System.out.println();
        }

        StringBuilder enc = new StringBuilder();

        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                if(railMatrix[i][j] != '\0'){
                    enc.append(railMatrix[i][j]);
                }
            }
        }

        return enc.toString();
    }

    public static String decrypt(int rails,String text){
        if(rails<=1){
            return text;
        }

        char[][] railMatrix = new char[rails][text.length()];

        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                railMatrix[i][j] ='\0';
            }
        }

        boolean down = false;
        int row = 0;

        for(int i=0;i<text.length();i++){
            if(row == 0 || row == rails-1){
                down = !down;
            }

            railMatrix[row][i] = '*';
            row += down ? 1 : -1;
        }

        int index = 0;
        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                if(railMatrix[i][j] == '*' && index<text.length()){
                    railMatrix[i][j] = text.charAt(index++);
                }
            }
        }

        System.out.println("Printing the decryption matrix........");
        for(int i=0;i<rails;i++){
            for(int j=0;j<text.length();j++){
                System.out.print(railMatrix[i][j] != '\n' ? railMatrix[i][j] + " " : " ");
            }
            System.out.println();
        }

        down = false;
        row = 0;

        StringBuilder dec = new StringBuilder();

        for(int i=0;i<text.length();i++){
            if(row == 0 || row == rails-1){
                down = !down;
            }

            dec.append(railMatrix[row][i]);
            row += down ? 1:-1;
        }
        return dec.toString();
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("Enter the text to encrypt: ");
        String text = sc.nextLine();

        System.out.println("Enter number of rails: ");
        int rails = sc.nextInt();

        String encrypted = encrypt(rails, text.toUpperCase());
        System.out.println();
        System.out.println("The encrypted text: "+encrypted);
        System.out.println();

        String decrypted = decrypt(rails, encrypted.toUpperCase());
        System.out.println();
        System.out.println("The Decrypted text: "+decrypted);
        System.out.println();

        sc.close();
    }
}











































/*import java.security.KeyPair;
import java.security.KeyPairGenerator;
import javax.crypto.Cipher;
import java.util.Scanner;


public class Encrypt{
    public static void main(String args[]) throws Exception {

        KeyPairGenerator keyPairGen = KeyPairGenerator.getInstance("RSA");

        keyPairGen.initialize(2048);

        KeyPair pair = keyPairGen.generateKeyPair();

        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");

        cipher.init(Cipher.ENCRYPT_MODE, pair.getPublic() );

        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter the text to encrypt:");
        String inputText = scanner.nextLine();

        byte[] input = inputText.getBytes();
        cipher.update(input);

        byte[] cipherText = cipher.doFinal();
        System.out.println(new String(cipherText, "UTF-8"));
    }
}  */







/*
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.PublicKey;
import java.util.Base64;
import java.util.Scanner;
import javax.crypto.Cipher;

public class Decrypt {
    public static void main(String args[]) throws Exception {

        KeyPairGenerator keyPairGen = KeyPairGenerator.getInstance("RSA");

        keyPairGen.initialize(2048);

        KeyPair pair = keyPairGen.generateKeyPair();

        PublicKey publicKey = pair.getPublic();

        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");

        cipher.init(Cipher.ENCRYPT_MODE, publicKey);

        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the text to be encrypted : ");
        String inputText = scanner.nextLine();

        byte[] input = inputText.getBytes();
        cipher.update(input);

        byte[] cipherText = cipher.doFinal();

        String encryptedText = Base64.getEncoder().encodeToString(cipherText);
        System.out.println("-----------------------------------------");
        System.out.println("Encrypted text : " + encryptedText);
        System.out.println("-----------------------------------------");

        cipher.init(Cipher.DECRYPT_MODE, pair.getPrivate());

        byte[] decodedCipherText = Base64.getDecoder().decode(encryptedText);

        byte[] decipheredText = cipher.doFinal(decodedCipherText);
        System.out.println("Decrypted Text: " + new String(decipheredText));

        
    }
    
} 
*/






/*
import java.util.*;

public class PlayFairCipher {
    static char[][] keyMatrix = new char[5][5];

    static void generateKeyMatrix(String key) {
        Set<Character> used = new HashSet<>();
        key = key.toUpperCase().replaceAll("[^A-Z]", "").replace("J", "I");
        StringBuilder sb = new StringBuilder();

        for (char c : key.toCharArray())
            if (used.add(c)) sb.append(c);

        for (char c = 'A'; c <= 'Z'; c++)
            if (c != 'J' && used.add(c)) sb.append(c);

        for (int i = 0, k = 0; i < 5; i++)
            for (int j = 0; j < 5; j++)
                keyMatrix[i][j] = sb.charAt(k++);
    }

    static String formatMessage(String msg) {
        msg = msg.toUpperCase().replaceAll("[^A-Z]", "").replace("J", "I");
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < msg.length(); ) {
            char a = msg.charAt(i++);
            char b = (i < msg.length()) ? msg.charAt(i) : 'X';
            if (a == b) b = 'X';
            else i++;
            sb.append(a).append(b);
        }
        if (sb.length() % 2 != 0) sb.append('X');
        return sb.toString();
    }

    static String encrypt(String text) {
        return processText(text, true);
    }

    static String decrypt(String text) {
        return processText(text, false);
    }

    static String processText(String text, boolean encrypt) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < text.length(); i += 2) {
            char a = text.charAt(i), b = text.charAt(i + 1);
            int[] posA = findPosition(a), posB = findPosition(b);
            int rowA = posA[0], colA = posA[1], rowB = posB[0], colB = posB[1];

            if (rowA == rowB) {
                colA = (colA + (encrypt ? 1 : 4)) % 5;
                colB = (colB + (encrypt ? 1 : 4)) % 5;
            } else if (colA == colB) {
                rowA = (rowA + (encrypt ? 1 : 4)) % 5;
                rowB = (rowB + (encrypt ? 1 : 4)) % 5;
            } else {
                int temp = colA;
                colA = colB;
                colB = temp;
            }

            result.append(keyMatrix[rowA][colA]).append(keyMatrix[rowB][colB]);
        }
        return result.toString();
    }

    static int[] findPosition(char ch) {
        for (int i = 0; i < 5; i++)
            for (int j = 0; j < 5; j++)
                if (keyMatrix[i][j] == ch)
                    return new int[]{i, j};
        return null;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter the key: ");
        String key = sc.nextLine();
        System.out.print("Enter the plain text: ");
        String text = sc.nextLine();

        generateKeyMatrix(key);
        String formatted = formatMessage(text);
        String encrypted = encrypt(formatted);
        String decrypted = decrypt(encrypted);

        System.out.println("Formatted: " + formatted);
        System.out.println("Encrypted: " + encrypted);
        System.out.println("Decrypted: " + decrypted);
    }
}
 */



