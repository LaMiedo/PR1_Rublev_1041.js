var desk = [];
var n = 8;
var cur_x = n / 2;
var cur_y = n / 2;

//main
$('document').ready(function () {
    //init mtx
    desk = new Array(n);
    for (var i = 0; i < n; i++) {
        desk[i] = new Array(n);
    }

    //processing
    while (true) {
        placeBishop(cur_y, cur_x, cur_x & 1 ? 2 : 1);
        if (!findFreeCell())
            break;
    }
    //output
    $.each(desk, function (key, value) {
        $('#result').append('<br>' + value);
    });
});


function checkCell(y, x) {
    if (desk[y][x] == null) {
        cur_x = x;
        cur_y = y;
        return true;
    }
    return false;
}

function findFreeCell() {
    for (var y = n / 2; y < n; y++)
        for (var x = n / 2; x < n; x++) {
            if (checkCell(y, x) ||
                checkCell(y, n - x - 1) ||
                checkCell(n - y - 1, n - x - 1) ||
                checkCell(n - y - 1, x))
                return true;
        }
    return false;
}

//Fill up free diagonal cells
function placeBishop(y, x, val) {
    desk[y][x] = 7 + val;
    $.each(desk, function (key, value) {
        var y1 = Math.abs(key - y);
        var x1 = x - y1;
        if (x1 >= 0 && (key != y || x1 != x))
            desk[key][x1] = val;
        x1 = x + y1;
        if (x1 < n && (key != y || x1 != x))
            desk[key][x1] = val;
    });
}
       
