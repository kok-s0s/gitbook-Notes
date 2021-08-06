# 回溯算法-04

## 重新安排行程

```javascript
/**
 * @param {string[][]} tickets
 * @return {string[]}
 */
var findItinerary = function (tickets) {
    let res = ['JFK'];
    let map = {};

    for (const ticket of tickets) {
        const [from, to] = ticket;
        if (!map[from])
            map[from] = [];
        map[from].push(to);
    }

    for (const city in map) {
        map[city].sort();
    }

    const backTracking = () => {
        if (res.length === tickets.length + 1)
            return true;

        if (!map[res[res.length - 1]] || !map[res[res.length - 1]].length)
            return false;

        for (let i = 0; i < map[res[res.length - 1]].length; ++i) {
            let city = map[res[res.length - 1]][i];

            map[res[res.length - 1]].splice(i, 1);

            res.push(city);

            if (backTracking())
                return true;

            res.pop();

            map[res[res.length - 1]].splice(i, 0, city);
        }
    }

    backTracking();

    return res;
};
```

## N皇后

```javascript
/**
 * @param {number} n
 * @return {string[][]}
 */
var solveNQueens = function (n) {
    let res = [];

    const transformChessBoard = (chessBoard) => {
        let chessBoardBack = [];
        chessBoard.forEach(row => {
            let rowStr = '';
            row.forEach(value => {
                rowStr += value;
            })
            chessBoardBack.push(rowStr);
        })
        return chessBoardBack;
    }

    const isValid = (row, col, chessBoard) => {
        for (let i = 0; i < row; ++i) {
            if (chessBoard[i][col] === 'Q')
                return false;
        }

        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) {
            if (chessBoard[i][j] === 'Q')
                return false;
        }

        for (let i = row - 1, j = col + 1; i >= 0 && j < n; --i, ++j) {
            if (chessBoard[i][j] === 'Q')
                return false;
        }

        return true;
    }

    const backTracking = (row, chessBoard) => {
        if (row === n) {
            res.push(transformChessBoard(chessBoard));
            return;
        }

        for (let col = 0; col < n; ++col) {
            if (isValid(row, col, chessBoard)) {
                chessBoard[row][col] = 'Q';
                backTracking(row + 1, chessBoard);
                chessBoard[row][col] = '.';
            }
        }
    }

    let chessBoard = new Array(n).fill([]).map(() => new Array(n).fill('.'));
    backTracking(0, chessBoard);

    return res;
};
```

## 解数独

```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
var solveSudoku = function (board) {
    const isValid = (row, col, val) => {
        for (let i = 0; i < board.length; ++i)
            if (board[row][i] === val || board[i][col] === val)
                return false;

        let tempRow = Math.floor(row / 3) * 3;
        let tempCol = Math.floor(col / 3) * 3;

        for (let i = tempRow; i < tempRow + 3; ++i) {
            for (let j = tempCol; j < tempCol + 3; ++j) {
                if (board[i][j] === val)
                    return false;
            }
        }

        return true;
    }

    const backTracking = () => {
        for (let i = 0; i < board.length; ++i) {
            for (let j = 0; j < board[0].length; ++j) {
                if (board[i][j] !== '.')
                    continue;
                for (let val = 1; val <= 9; ++val) {
                    if (isValid(i, j, `${val}`)) {
                        console.log(val);
                        console.log(`${val}`)
                        board[i][j] = `${val}`;
                        if (backTracking()) {
                            return true;
                        }
                        board[i][j] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }

    backTracking();

    return board;
};
```

