import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Scanner;


public class LifeGame {
	private MySparseArray curGen = new MySparseArray(0);
	private MySparseArray numNeighbors = new MySparseArray(0);
	private MySparseArray nextGen = new MySparseArray(0);

	public LifeGame(Scanner sc){
		sc.useDelimiter("\\D+");
		load(sc);
	}

	private void load(Scanner sc){
		int xCoordinate;
		int yCoordinate;
		while(sc.hasNext()){
			xCoordinate = sc.nextInt();
			yCoordinate = sc.nextInt();
			curGen.setValue(xCoordinate, yCoordinate, 1);
		}
	}

	private SparseArray countNeighbors(){
		RowIterator iterate = curGen.iterateRows();
		numNeighbors = new MySparseArray(0);
		while(iterate.hasNext()){
			ElemIterator rowElemIterator = iterate.next(); 
			while(rowElemIterator.hasNext()){
				MatrixElem elem = rowElemIterator.next();
				int rowIndex = elem.rowIndex();
				int colIndex = elem.columnIndex();
				int curCount;
				if(rowIndex > 1){
					if(colIndex > 1){
						curCount = (Integer) numNeighbors.elementAt((rowIndex - 1), (colIndex -1));
						numNeighbors.setValue((rowIndex - 1), (colIndex - 1), curCount + 1);
					}
					curCount = (Integer) numNeighbors.elementAt((rowIndex - 1), (colIndex));
					numNeighbors.setValue((rowIndex - 1), (colIndex), curCount + 1);
					curCount = (Integer) numNeighbors.elementAt((rowIndex -1), (colIndex+1));
					numNeighbors.setValue((rowIndex - 1), (colIndex + 1), curCount + 1);
				}
				if(colIndex > 1){
					curCount = (Integer) numNeighbors.elementAt((rowIndex), colIndex - 1);
					numNeighbors.setValue((rowIndex), colIndex -1,curCount + 1);
				}
				curCount = (Integer) numNeighbors.elementAt((rowIndex), colIndex + 1);
				numNeighbors.setValue((rowIndex), colIndex + 1,curCount + 1);
				//row3
				if(colIndex > 1){
					curCount = (Integer) numNeighbors.elementAt((rowIndex + 1), colIndex - 1);
					numNeighbors.setValue((rowIndex + 1), colIndex -1,curCount + 1);
				}
				curCount = (Integer) numNeighbors.elementAt((rowIndex + 1), colIndex);
				numNeighbors.setValue((rowIndex + 1), colIndex,curCount + 1);
				curCount = (Integer) numNeighbors.elementAt((rowIndex + 1), colIndex + 1);
				numNeighbors.setValue((rowIndex + 1), colIndex + 1,curCount + 1);
			}
		}
		return numNeighbors;
	}

	public void nextGeneration(){
		countNeighbors();
		nextGen = new MySparseArray(0);
		RowIterator iterate = numNeighbors.iterateRows();
		while(iterate.hasNext()){
			ElemIterator rowElemIterator = iterate.next(); 
			while(rowElemIterator.hasNext()){
				MatrixElem elem = rowElemIterator.next();
				int rowIndex = elem.rowIndex();
				int colIndex = elem.columnIndex();
				int curCount = (Integer) numNeighbors.elementAt(rowIndex , colIndex);
				if(curCount == 3){
					nextGen.setValue(rowIndex, colIndex, 1);
				}
				else if(curCount == 2){ //if alive, keep it alive for next round
					if(((Integer)(curGen.elementAt(rowIndex, colIndex))).compareTo(0) != 0){
						nextGen.setValue(rowIndex, colIndex, 1);
					} 
				}
			}
		}
		curGen = nextGen;
	}
	public void writeFile(String outputPath){
		try {
			PrintWriter write = new PrintWriter(new BufferedWriter(new FileWriter(outputPath)));
			RowIterator iterate = curGen.iterateRows();
			while(iterate.hasNext()){
				ElemIterator rowElemIterator = iterate.next(); 
				while(rowElemIterator.hasNext()){
					MatrixElem elem = rowElemIterator.next();
					int rowIndex = elem.rowIndex();
					int colIndex = elem.columnIndex();
					write.print(rowIndex + "," + colIndex + "\n");
					System.out.print(rowIndex);
					System.out.println("," + colIndex);
				}
			}
			write.flush();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} 
	}
}
