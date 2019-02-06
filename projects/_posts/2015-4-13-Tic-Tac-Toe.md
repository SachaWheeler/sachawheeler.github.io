---
layout: project
title: Tic Tac Toe - A Gamebook
image: /images/tictactoe/cover.png
summary: A gamebook where you play tic-tac-toe against the book.
---

I've always liked choose-your-own-adventure-style gamebooks, and wanted to write one. I started with Chess and checkers, but realised the book would be rather large.

I settled on Tic-Tac-Toe.

![](/images/tictactoe/cover.png)

First, I need to teach it to play the game. (In those days, php seemed a simple option...)
```php
<?php
$start = "000000000";
$tree = array();
$pages = array($start);
$page_pointers = array();

// first and second moves
$tree[$start] = addChildren($start);
?>
```

the addChildren function takes a gameboard, and makes a move

```php
function addChildren($board){
    global $pages, $page_pointers;
    $children = array();
    $done = array();
    $board_array = str_split($board);
    for($x=0 ; $x<count($board_array) ; $x++)
    {
        if(in_array($x, $done) || $board_array[$x] != '0') continue;
        $done[] = $x;
        $p1move = substr_replace($board, '1', $x, 1);
        if(isWinner($p1move, 1)){
            $children[$p1move] = "win";
            if(!in_array($p1move, $pages)) $pages[] = $p1move;
            continue;
        }
        if(substr_count($p1move, '0') == 0){
            $children[$p1move] = "tie";
            if(!in_array($p1move, $pages)) $pages[] = $p1move;
            continue;
        }
        $p2move = get_move($p1move);
        if(isWinner($p2move, 2)){
            $children[$p2move] = "lose";
            if(!in_array($p2move, $pages)) $pages[] = $p2move;
            $page_pointers[$p1move] = array_search($p2move, $pages);
            continue;
        }
        $children[$p2move] = 'x';
        if(!in_array($p2move, $pages)) $pages[] = $p2move;
        $page_pointers[$p1move] = array_search($p2move, $pages);
        // $page_pointers[$board][$x] = key( array_slice( $pages, -1, 1, TRUE ) );
    }
    return $children;
}
```

which I call, rather inefficiently, but this programme was only ever intended to be run once.

```php
// first and second moves
$tree[$start] = addChildren($start);
// third and fourth moves
foreach($tree[$start] as $k => $v){
    if($v == 'x')
        $tree[$start][$k] = addChildren($k);
}
// fifth and sixth moves
foreach($tree[$start] as $k => $v){
    foreach($v as $k2 => $v2){
        if($v2 == 'x')
            $tree[$start][$k][$k2] = addChildren($k2);
    }
}
// seventh and eigth moves
foreach($tree[$start] as $k => $v){
    foreach($v as $k2 => $v2){
        foreach($v2 as $k3 => $v3){
            if($v3 == 'x')
                $tree[$start][$k][$k2][$k3] = addChildren($k3);
        }
    }
}
foreach($tree[$start] as $k => $v){
    foreach($v as $k2 => $v2){
        foreach($v2 as $k3 => $v3){
            if(is_array($v3)){
                foreach($v3 as $k4 => $v4){
                    if($v4 == 'x')
                        $tree[$start][$k][$k2][$k3][$k4] = addChildren($k4);
                }
            }
        }
    }
}
foreach($tree[$start] as $k => $v){
    foreach($v as $k2 => $v2){
        foreach($v2 as $k3 => $v3){
            if(is_array($v3)){
                foreach($v3 as $k4 => $v4){
                    if(is_array($v4)){
                        foreach($v4 as $k5 => $v5){
                            if($v5 == 'x')
                                $tree[$start][$k][$k2][$k3][$k4][$k5] = addChildren($k5);
                        }
                    }
                }
            }
        }
    }
}
```

Making a move is quite simple for the book. It looks to see if it (the book) can win, and if so, it tries to.


```php
function get_move($board){
    // echo "board: {$board}\n";
    $b = str_split($board);
    // try to win
    // 0 represents and empty square
    // 1 represnts a player's square (X)
    // 2 represnts a book's square (O)
    if($b[0] == 0 && (($b[1] == 2 && $b[2] == 2) ||
                      ($b[3] == 2 && $b[6] == 2) ||
                      ($b[4] == 2 && $b[8] == 2)))
        $move = 0;
    elseif($b[1] == 0 && (($b[0] == 2 && $b[2] == 2) ||
                          ($b[4] == 2 && $b[7] == 2)))
        $move = 1;
    elseif($b[2] == 0 && (($b[0] == 2 && $b[1] == 2) ||
                          ($b[4] == 2 && $b[6] == 2) ||
                          ($b[5] == 2 && $b[8] == 2)))
        $move = 2;
    elseif($b[3] == 0 && (($b[0] == 2 && $b[6] == 2) ||
                          ($b[4] == 2 && $b[5] == 2)))
        $move = 3;
    elseif($b[4] == 0 && (($b[0] == 2 && $b[8] == 2) ||
                          ($b[2] == 2 && $b[6] == 2) ||
                          ($b[1] == 2 && $b[7] == 2) ||
                          ($b[3] == 2 && $b[5] == 2)))
        $move = 4;
    elseif($b[5] == 0 && (($b[2] == 2 && $b[8] == 2) ||
                          ($b[3] == 2 && $b[4] == 2)))
        $move = 5;
    elseif($b[6] == 0 && (($b[0] == 2 && $b[3] == 2) ||
                          ($b[4] == 2 && $b[2] == 2) ||
                          ($b[7] == 2 && $b[8] == 2)))
        $move = 6;
    elseif($b[7] == 0 && (($b[1] == 2 && $b[4] == 2) ||
                          ($b[6] == 2 && $b[8] == 2)))
        $move = 7;
    elseif($b[8] == 0 && (($b[0] == 2 && $b[4] == 2) ||
                          ($b[6] == 2 && $b[7] == 2) ||
                          ($b[2] == 2 && $b[5] == 2)))
        $move = 8;
```

Then it looks to see if the player is about to win, and tries to block it.
```php
    elseif($b[0] == 0 && (($b[1] == 1 && $b[2] == 1) ||
                          ($b[3] == 1 && $b[6] == 1) ||
                          ($b[4] == 1 && $b[8] == 1)))
        $move = 0;
    elseif($b[1] == 0 && (($b[0] == 1 && $b[2] == 1) ||
                          ($b[4] == 1 && $b[7] == 1)))
        $move = 1;
    elseif($b[2] == 0 && (($b[0] == 1 && $b[1] == 1) ||
                          ($b[4] == 1 && $b[6] == 1) ||
                          ($b[5] == 1 && $b[8] == 1)))
        $move = 2;
    elseif($b[3] == 0 && (($b[0] == 1 && $b[6] == 1) ||
                          ($b[4] == 1 && $b[5] == 1)))
        $move = 3;
    elseif($b[4] == 0 && (($b[0] == 1 && $b[8] == 1) ||
                          ($b[2] == 1 && $b[6] == 1) ||
                          ($b[1] == 1 && $b[7] == 1) ||
                          ($b[3] == 1 && $b[5] == 1)))
        $move = 4;
    elseif($b[5] == 0 && (($b[2] == 1 && $b[8] == 1) ||
                          ($b[3] == 1 && $b[4] == 1)))
        $move = 5;
    elseif($b[6] == 0 && (($b[0] == 1 && $b[3] == 1) ||
                          ($b[4] == 1 && $b[2] == 1) ||
                          ($b[7] == 1 && $b[8] == 1)))
        $move = 6;
    elseif($b[7] == 0 && (($b[1] == 1 && $b[4] == 1) ||
                          ($b[6] == 1 && $b[8] == 1)))
        $move = 7;
    elseif($b[8] == 0 && (($b[0] == 1 && $b[4] == 1) ||
                          ($b[6] == 1 && $b[7] == 1) ||
                          ($b[2] == 1 && $b[5] == 1)))
        $move = 8;

```

Otherwise, if it can't win, and you can't win, *and* if the centre square is empty, it goes for the centre square

```php

    // try the centre square
    elseif( $b[4] == 0)
        $move = 4;
```

Otherwise it chooses a "random" square, which is impossible, so it chooses the first empty square

```php
    else
        $move = strpos($board, '0');
```

By now, I've saved an array of pages. Each has a gameboard, with book and player moves.

Each square or each board has either a "X", and "O", a page number (which represents the board that you would have if you were to play that square), or is blank if the game has already been won (or lost).

![](/images/tictactoe/page_1.png)

So, now to export the pages as image files.

First, draw the board, with gridlines

```php
function save_image($board_string){
    // Create an image
    global $width, $height, $line_width, $offset, $font_path, $page_font_size, $result_size, $pages;
    global $page_pointers, $image_count;
    $image_count++;
    $im = imagecreatetruecolor($width, $height);
    $white = imagecolorallocate($im, 0xFF, 0xFF, 0xFF);
    $grey = imagecolorallocate($im, 0xbd, 0xbd, 0xbd);
    $black = imagecolorallocate($im, 0x00, 0x00, 0x00);
    $red = imagecolorallocate($im, 255, 0, 0);

    imagecolortransparent($im, $white);

    // Set the background to be white
    imagefilledrectangle($im, 0, 0, $width, $height, $white);

    // Set the line thickness
    imagesetthickness($im, $line_width);

    // draw the grid
    $grid = array(
            array(0, $width/3, $width, $width/3),
            array(0, ($width/3)*2, $width, ($width/3)*2),
            array($width/3, 0, $width/3, $width),
            array(($width/3)*2, 0, ($width/3)*2, $width),
        );
    foreach($grid as $line)
        imageline ( $im , $line[0] , $line[1] , $line[2] , $line[3] , $black );
```

Then, determine if someone has already won

```php
    if(isWinner($board_string, 1))
        $state = "win";
    elseif(isWinner($board_string, 2))
        $state = "lose";
    elseif(substr_count($board_string, 0) == 0)
        $state = "tie";
    else
        $state = "play";
```

The isWinner function is self-explanatory

```php
function isWinner($board, $player){
    $b = str_split($board);
    if(
        ($b[0] == $player && $b[1] == $player && $b[2] == $player ) ||
        ($b[3] == $player && $b[4] == $player && $b[5] == $player ) ||
        ($b[6] == $player && $b[7] == $player && $b[8] == $player ) ||
        ($b[0] == $player && $b[3] == $player && $b[6] == $player ) ||
        ($b[1] == $player && $b[4] == $player && $b[7] == $player ) ||
        ($b[2] == $player && $b[5] == $player && $b[8] == $player ) ||
        ($b[0] == $player && $b[4] == $player && $b[8] == $player ) ||
        ($b[2] == $player && $b[4] == $player && $b[6] == $player ))
    return true;
}

```

![](/images/tictactoe/in-play.png)

In drawing the Xs and Os on the board, we also determine what page number reprents each available move

```php
switch($board[$square]){
            case 0: // the square is empty
                if($state == "play"){ // draw the page number if the game hasn't been won
                    // get the page number
                    $dest_page_board = substr_replace($board_string, '1', $square, 1);
                    // print_r($board_string); echo "\n"; print_r($dest_page_board); die(0);
                    if(in_array($dest_page_board, $pages))
                        $text = array_search($dest_page_board, $pages)+1;
                    elseif(isset($page_pointers[$dest_page_board]))
                        $text = $page_pointers[$dest_page_board]+1;
                    else
                        $text = "page #";
                    $bb = imagettfbbox ( $page_font_size , 0 , $font_path , $text );
                    $half_text_width = ($bb[2] - $bb[0]) /2;
                    $half_text_height = ($bb[7] - $bb[1]) /2;
                    imagettftext($im, $page_font_size, 0, $x - $half_text_width, $y - $half_text_height, $black, $font_path, $text);
                }
                break;
            case 1: // draw an 'x'
                imageline( $im , $x-$offset, $y-$offset, $x+$offset, $y+$offset, $grey);
                imageline( $im , $x-$offset, $y+$offset, $x+$offset, $y-$offset, $grey);
                break;
            case 2: // draw an 'o'
                // imageellipse ( $im , $x , $y , $offset*2 , $offset*2 , $grey );
                imagefilledellipse  ($im , $x , $y , ($offset+$line_width)*2 , ($offset+$line_width)*2 , $grey);
                imagefilledellipse  ($im , $x , $y ,$offset*2 ,$offset*2 ,$white);
                break;
        }
```

![](/images/tictactoe/win-lose.png)

If it's a tie, just end there. If it's a win or loss, show the winning move

```php
    if($state == "tie"){
        // tie
        $state = "it's a tie";
        $bb = imagettfbbox ( $result_size , 0 , $font_path , $state );
        $half_text_width = ($bb[2] - $bb[0]) /2;
        imagettftext($im, $result_size, 0, $width/2 - $half_text_width, $width+($height-$width)/2, $red, $font_path, $state);
    }elseif($state != "play"){
        $state = "you ".$state;
        $coords = getwinningline($board);
        $start = getcoordinates($coords[0]);
        $end = getcoordinates($coords[1]);
        if($coords[2] == 'h'){
            $start[0] -= ($offset/2);
            $end[0]   += ($offset/2);
        }elseif($coords[2] == 'v'){
            $start[1] -= ($offset/2);
            $end[1]   += ($offset/2);
        }elseif($coords[2] == 'd1'){
            $start[0] -= ($offset/2);
            $start[1] -= ($offset/2);
            $end[0]   += ($offset/2);
            $end[1]   += ($offset/2);
        }elseif($coords[2] == 'd2'){
            $start[0] -= ($offset/2);
            $start[1] += ($offset/2);
            $end[0]   += ($offset/2);
            $end[1]   -= ($offset/2);
        }
        imagesetthickness($im, $line_width*2);
        imageline( $im , $start[0], $start[1], $end[0], $end[1], $red);

        $bb = imagettfbbox ( $result_size , 0 , $font_path , $state );
        $half_text_width = ($bb[2] - $bb[0]) /2;
        imagettftext($im, $result_size, 0, $width/2 - $half_text_width, $width+($height-$width)/2, $red, $font_path, $state);
    }

    imagepng($im, "./results/".sprintf("%03d", $image_count)."-".$board_string.".png");
```

![](/images/tictactoe/tie-win.png)

Finally the images are saved, sequentially names, in a folder which I uploaded to Blurb.com who print it.

## Thoughts
Lots of fun. I was particularly happy with the way I "reuse" identical boards to save on the total number of pages.

Most board states can be arrived at via different paths - there is no need to print multiple identical pages.

The unit cost at Blurb.com is way too high for a book like this - one day I'll get around to printing it properly.

The full source is [here](https://github.com/SachaWheeler/tic-tac-toe)
