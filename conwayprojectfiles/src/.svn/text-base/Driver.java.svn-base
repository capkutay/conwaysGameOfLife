import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;


public class Driver {
	public static void main(String[] args){
		String initialPath = args[0];
		String outputPath = args[1];
		String intString = args[2];
		int numGenerations = Integer.parseInt(intString);
		Scanner sc = null;
		try {
			sc = new Scanner(new File(initialPath));
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		LifeGame life = new LifeGame(sc);
		for(int i = 0; i < numGenerations; i++){
			life.nextGeneration();
		}
		life.writeFile(outputPath);
	}
}
