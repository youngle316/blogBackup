# Day 30 回溯算法 - N皇后

## 51\. N皇后

```typescript
function solveNQueens(n: number): string[][] {
	let result = [];
	
	const isValid = (col: number, row: number, chessBoard: string[][]) => {
		for(let i = 0; i < row; i++) {
			if(chessBoard[i][col] === 'Q') {
				return false
			}
		}
		
		for(let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
			if(chessBoard[i][j] === 'Q') {
				return false
			}
		} 
		
		for(let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
			if(chessBoard[i][j] === 'Q') {
				return false
			}
		}
		return true
	};
	
	const transformChessBoard = (chessBoard) => {
		let chessBoardBack = []
		chessBoard.forEach(row => {
			let rowStr = ''
			row.forEach(value => {
				rowStr += value
			})
			chessBoardBack.push(rowStr)
		})
		return chessBoardBack
	} 
	
	const backtracking = (row, chessBoard) => {
		if (row === n) {
			result.push(transformChessBoard(chessBoard));
			return;
		}
		for (let col = 0; col < n; col++) {
			if (isValid(col, row, chessBoard)) {
				chessBoard[row][col] = 'Q';
				backtracking(row + 1, chessBoard);
				chessBoard[row][col] = '.';
			}
		}
	};
	
	let chessBoard = new Array(n).fill([]).map(() => new Array(n).fill('.'))
	backtracking(0, chessBoard);
	return result;
};
```