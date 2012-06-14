
public class MyMatrixElem implements MatrixElem {
	private MyMatrixElem nextRowNode;
	private MyMatrixElem nextColNode;
	private int rowIndex;
	private int colIndex;
	private Object value;
	
	public MyMatrixElem(int rowIndex, int colIndex, Object value){
		this.rowIndex = rowIndex;
		this.colIndex = colIndex;
		this.value = value;
	}
	public int rowIndex() {
		
		return rowIndex;
	}
	public int columnIndex() {
		return colIndex;
	}
	public Object value() {
		return value;
	}
	
	public MyMatrixElem nextRowNode(){
		return nextRowNode;
	}
	
	public MyMatrixElem nextColNode(){
		return nextColNode;
	}
	
	public void setValue(Object value){
		this.value = value;
	}
	
	public void setNextRow(MyMatrixElem nextRowNode){
		this.nextRowNode = nextRowNode;
	}
	
	public void setNextCol(MyMatrixElem nextColNode){
		this.nextColNode = nextColNode;
	}
	
}
