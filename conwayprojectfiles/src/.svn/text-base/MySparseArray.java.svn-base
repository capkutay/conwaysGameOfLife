
public class MySparseArray implements SparseArray {

	Object defaultValue;
	RowColNode firstRowNode;
	RowColNode firstColNode;

	public MySparseArray(Object defaultValue){
		this.defaultValue = defaultValue;
	}

	public Object defaultValue() {
		return defaultValue;
	}
	/*
	 * Element Iterator inner class 
	 */

	class MyElemIterator extends ElemIterator {
		MyMatrixElem cur; 
		boolean isRow;
		int index;

		MyElemIterator(MyMatrixElem cur, boolean isRow, int iteratingIndex){
			this.cur = cur;
			this.isRow = isRow;
			this.index = iteratingIndex;
		}

		public boolean iteratingRow() {
			return isRow;
		}

		public boolean iteratingCol() {
			return !isRow;
		}

		public int nonIteratingIndex() {
			return index;
		}

		public MatrixElem next() {
			MyMatrixElem tmp = cur;
			if(isRow){
				cur = cur.nextColNode();
			} else{
				cur = cur.nextRowNode();
			}
			return tmp;
		}
		public boolean hasNext() {
			return cur != null;
		}
	}
	public RowIterator iterateRows() {
		class MyRowIterator extends RowIterator{
			RowColNode curRow; 
			MyRowIterator(){
				curRow = firstRowNode;
			}
			public ElemIterator next() {
				if(curRow == null){
					return null;
				} 
				RowColNode tmp = curRow;
				curRow = curRow.getNext();
				return new MyElemIterator(tmp.getElem(), true, tmp.getIndex());
			}
			public boolean hasNext() {
				return curRow != null;
			}
		}
		return new MyRowIterator();
	}

	public ColumnIterator iterateColumns() {
		class MyColumnIterator extends ColumnIterator{
			RowColNode curCol;
			MyColumnIterator(){
				curCol = firstColNode;
			}
			public ElemIterator next() {
				if(curCol == null){
					return null;
				}
				RowColNode tmp = curCol;
				curCol = curCol.getNext();
				return new MyElemIterator(tmp.getElem(), false, tmp.getIndex());
			}
			public boolean hasNext() {
				return curCol != null;
			}
		}
		return new MyColumnIterator();
	}

	public Object elementAt(int row, int col) {
		//walk through list of rows
		RowColNode elemRow = firstRowNode;
		while(elemRow != null && elemRow.getIndex() < row){
			elemRow = elemRow.getNext();
		}
		if(elemRow == null || elemRow.getIndex() != row){
			return defaultValue;
		}
		MyMatrixElem elem = elemRow.getElem();

		while(elem != null && elem.columnIndex() < col){
			elem = elem.nextColNode();
		}
		if(elem == null || elem.columnIndex() != col){
			return defaultValue;
		}
		return elem.value();
	}

	public void setValue(int row, int col, Object value) {
		if(elementAt(row, col) == value){
			return;
		}
		if (value == defaultValue)
			removeOldValue(row, col);
		else
			addNewValue(row, col, value);
	}	

	private void removeOldValue(int row, int col){
		
		//row and col nodes exist, matrix elem exists
		//find nodes, update elem pointers, update rowcolnode pointers and 
		RowColNode curRowNode = firstRowNode;
		RowColNode curColNode = firstColNode;
		RowColNode prevRowNode = null;
		RowColNode prevColNode = null;
		MyMatrixElem prevColElem = null;
		MyMatrixElem prevRowElem = null;
		
		while(curRowNode.getIndex() < row ){
			prevRowNode = curRowNode;
			curRowNode = curRowNode.getNext();
		} //got rowNode and prev row node
		MyMatrixElem firstRowElem = curRowNode.getElem();
		
		while(firstRowElem.columnIndex() < col){
			prevRowElem = firstRowElem;
			firstRowElem = firstRowElem.nextColNode();
		}
		
		while(curColNode.getIndex() < col){
			prevColNode = curColNode;
			curColNode = curColNode.getNext();
		}

		MyMatrixElem firstColElem = curColNode.getElem();
		while(firstColElem.rowIndex() < row){
			prevColElem = firstColElem;
			firstColElem = firstColElem.nextRowNode();
		}
		if(firstColElem != firstRowElem){
			System.out.println("Error: Remove Old Value Inconsistent.");
			System.exit(1);
		}
		//deal with row elems and row nodes
		if(prevRowElem != null){
			prevRowElem.setNextCol(firstRowElem.nextColNode());
		} else if(firstRowElem.nextColNode() != null){
			curRowNode.setMatrixElem(firstRowElem.nextColNode());
		} else if(prevRowNode != null){
			prevRowNode.setNext(curRowNode.getNext());
		} else{
			firstRowNode = curRowNode.getNext();
		}
		
		//col elems and col nodes
		if(prevColElem != null){
			prevColElem.setNextRow(firstColElem.nextRowNode());
		} else if(firstColElem.nextRowNode() != null){
			curColNode.setMatrixElem(firstColElem.nextRowNode());
		} else if(prevColNode != null){
			prevColNode.setNext(curColNode.getNext());
		} else{
			firstColNode = curColNode.getNext();
		}
		
	}

	private RowColNode findCreateRow(int row){
		RowColNode curRowNode = firstRowNode;
		RowColNode prevRowNode = null;
		//walk through rowNodes until rowNode is either null or greater than the row index
		while(curRowNode != null && curRowNode.getIndex() < row){
			prevRowNode = curRowNode;
			curRowNode = curRowNode.getNext();
		}
		if(curRowNode != null && curRowNode.getIndex() == row){
			return curRowNode;
		}
		RowColNode newRowNode = new RowColNode(row);
		if(prevRowNode != null){
			newRowNode.setNext(prevRowNode.getNext());
			prevRowNode.setNext(newRowNode);
		} else{ //beginning of the list of rows
			newRowNode.setNext(firstRowNode);
			firstRowNode = newRowNode;
		}
		return newRowNode;
	}

	private RowColNode findCreateCol(int col){
		RowColNode curColNode = firstColNode;
		RowColNode prevColNode = null;
		//walk through colNodes until colNode is either null or greater than the col index
		while(curColNode != null && curColNode.getIndex() < col){
			prevColNode = curColNode;
			curColNode = curColNode.getNext();
		}
		if(curColNode != null && curColNode.getIndex() == col){
			return curColNode;
		}
		RowColNode newColNode = new RowColNode(col);
		if(prevColNode != null){
			newColNode.setNext(prevColNode.getNext());
			prevColNode.setNext(newColNode);
		} else{ //beginning of the list of rows
			newColNode.setNext(firstColNode);
			firstColNode = newColNode;
		}
		return newColNode;
	}

	private void addNewValue(int row, int col, Object value){
		MyMatrixElem newElem;
		RowColNode rowNode = findCreateRow(row);
		//walk through row of matrix elements 
		MyMatrixElem curElem = rowNode.getElem();
		MyMatrixElem prevElem = null;

		while(curElem != null && curElem.columnIndex() < col){
			prevElem = curElem;
			//calling next matrix element HORIZONTALLY
			// newElem ======> nextColElement
			curElem = curElem.nextColNode();
		} 
		if(curElem != null && curElem.columnIndex() == col){
			curElem.setValue(value);
			return;
		}
		newElem = new MyMatrixElem(row, col, value);
		//setting next matrix element HORIZONTALLY
		// newElem ======> nextColElement
		newElem.setNextCol(curElem);
		
		if(prevElem != null){
			prevElem.setNextCol(newElem);
		} else{ 
			rowNode.setMatrixElem(newElem);
		}
		RowColNode colNode = findCreateCol(col);

		//walk through col of matrix elements 
		curElem = colNode.getElem();
		prevElem = null;

		while(curElem != null && curElem.columnIndex() < row){
			prevElem = curElem;
			//calling next matrix element VERTICALLY down sparse array
			//              |curElem|
			//				   | |
			//                 | |
			//                 | | 
			//                  V
			//             |nextRowElem|
			curElem = curElem.nextRowNode();
		} 
		newElem.setNextRow(curElem);
		if(prevElem != null){
			prevElem.setNextRow(newElem);
		} else{ 
			colNode.setMatrixElem(newElem);
		}
	}
	//test

}

