勤怠

  <HEAD>
    <TITLE></TITLE>
    <STYLE type="text/css">
      <!--
      BODY {
        background-image: url("gazou/haikei.jpg"); /* 全体の背景画像 */
        background-repeat: no-repeat;            /* 背景を繰り返さない */
        background-position: 50% 50%;            /* 背景画像の位置は画面中央 */
        background-attachment:fixed;             /* 背景画像を固定する */
        background-size:2000px auto;
      }
    </STYLE>
  </HEAD>
  <BODY>
  </BODY>
</HTML>




<?php
ini_set('display_errors', 'On');
error_reporting(E_ALL);

$dsn = 'mysql:dbname=mfdb;host=localhost';
$user = 'root';
$password = '';
//日本語の曜日配列
$weekjp = array(
  '日', //0
  '月', //1
  '火', //2
  '水', //3
  '木', //4
  '金', //5
  '土'  //6
  );

//現在の曜日番号（日:0  月:1  火:2  水:3  木:4  金:5  土:6）を取得
$weekno = date('w');


try {

  if (filter_input_array(INPUT_POST)) {
$id = filter_input(INPUT_POST, 'id');//利用者ID
$first_name = filter_input(INPUT_POST, 'first_name');//利用者名
$startyear = filter_input(INPUT_POST, 'startyear');//今年度の開始年
$startmonth = filter_input(INPUT_POST, 'startmonth');//今年度の開始月
$startday = filter_input(INPUT_POST, 'startday');//今年度の開始日
$startweek = filter_input(INPUT_POST, 'startweek');//今年度の開始曜日
$startsituation = filter_input(INPUT_POST, 'startsituation');//勤怠内容
$field2 = filter_input(INPUT_POST, 'field2');//出勤時間

$err = array();//エラー内容



// エラーの数を数えて、エラーがなければインサート
if (0 == count($err)) {
//DB連携とDB接続できなくばエラー
  $db = new PDO($dsn, $user, $password, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));

  if (isset($_POST["delete"])) {
    $did = $_POST["delete"];
    $qry = "DELETE FROM attendanse WHERE attendanceid =:did";
    $stmt = $db->prepare($qry);
    $stmt->execute();
  }else if (isset($_POST["insert"])) {
    $did = $_POST["insert"];
    //attendanse（出勤用テーブル）を追加
    $sql = "INSERT INTO attendanse (";
    $sql .= "id, first_name, startyear, startmonth, startday, startweek,startsituation,field2 ";
    $sql .= ") VALUES (";
    $sql .= ":id,:first_name,:startyear,:startmonth,:startday,:startweek,:startsituation,:field2";
    $sql .= ")";
    $stmt = $db->prepare($sql);

    // パラメータのセットの仕方はマニュアルで確認してください。
    $stmt->bindParam(':id', $id, PDO::PARAM_STR);
    $stmt->bindParam(':first_name', $first_name, PDO::PARAM_INT);
    $stmt->bindParam(':startyear', $startyear, PDO::PARAM_STR);
    $stmt->bindParam(':startmonth', $startmonth, PDO::PARAM_STR);
    $stmt->bindParam(':startday', $startday, PDO::PARAM_STR);
    $stmt->bindParam(':startweek', $startweek, PDO::PARAM_STR);
    $stmt->bindParam(':startsituation', $startsituation, PDO::PARAM_STR);
    $stmt->bindParam(':field2', $field2, PDO::PARAM_STR);
    // 実行
    $stmt->execute();
  }

  $qry = "SELECT * FROM attendanse";
  $data = $db->query($qry);
  $data->execute();
  $rows = $data->fetchAll(PDO::FETCH_ASSOC);
//$registered = mysqli_affected_rows($db);
  echo "<P>下記表の内容で登録しました</P>";
}
}else {
  echo "<P>出勤時に必ず行って下さい</P>";
}
} catch (PDOException $e) {
  $err['all'] = $e->getMessage();
}
?>

<?php if (isset($err) && 0 < count($err)) : ?>
  <p><?= nl2br(implode(PHP_EOL, $err)); ?></p>
<?php endif; ?>
<div class="example">
  <form name="form1"action="Attendance.php" method="post">
    <table>
      <caption>勤怠登録手続き</caption>
      <tr>
        <th rowspan="2">従業員番号</th>
        <th rowspan="2">名前</th>
        <th colspan="4">日付</th>
        <th rowspan="2">時間</th>
        <th rowspan="2">出勤状況</th>
        <th rowspan="2">登録内容</th>
      </tr>
      <tr>
        <th>年</th>
        <th>月</th>
        <th>日</th>
        <th>曜日</th>
      </tr>
      <tr>
        <td><input type="text" name="id" size=" 10" maxlength="50" placeholder="○○太郎"
          value="<?php if (isset($_POST['id'])) {echo htmlspecialchars($_POST['id'],ENT_QUOTES,"UTF-8");} ?>"></td>
          <td><input type="text" name="first_name" size=" 10" maxlength="50" placeholder="○○太郎"
            value="<?php if (isset($_POST['first_name'])) {echo htmlspecialchars($_POST['first_name'],ENT_QUOTES,"UTF-8");} ?>"></td>
            <td><input type="text" name="startyear"  size="1"value="<?php echo date("Y"); ?>" /></td>
            <td><input type="text" name="startmonth"  size="1"value="<?php echo date("m"); ?>" /></td>
            <td><input type="text" name="startday"   size="1"value="<?php echo date("d"); ?>" /></td>
            <td><input type="text" name="startweek"  size="1"value="<?php echo $weekjp[$weekno]; ?>"></td>
            <td><input type="text" name="field2" size="9" value=""></td>
            <td><select name="startsituation">
              <option value="出 勤">出勤</option>
              <option value="退 勤"退勤</option>
                <option value="外 出">外出</option>
                <option value="戻 り">戻り</option>
              </select>
            </td>
            <td><input type="submit" value="出勤(in）" name="insert"></td>
          </tr>
        </table>
      </div>
      <hr><!-- //登録票と登録結果の区切り線?-->

      <form action="Attendance.php" method="post">

        <?php
//exitwork.php(退勤登録)の結果
//エラー設定
        ini_set('display_errors', 'On');
        error_reporting(E_ALL);
//DB接続設定

        try {


          echo "<div class='dbs'>
          <table border>
            <caption>出勤登録結果</caption>
            <tbody>
              <tr>
                <th>編集</th>
                <th>削除</th>
                <th>出勤番号</th>
                <th>会員番号</th>
                <th>名前</th>
                <th>出勤状況</th>
                <th>日付</th>
                <th>時間</th>
              </tr>
              <tr>";

                foreach ($rows as $row) {
                  echo "<td><a href='Attendancelist.php?attendanceid =".$row['attendanceid']."&=id".$row['id']."&first_name=".$row['first_name'].
                  "&startyear=".$row['startyear']."&startmonth=".$row['startmonth']."&startday=".$row['startday']."&startweek=".$row['startweek'].
                  "&startsituation=".$row['startsituation']."&field2=".$row['field2']."'>編集</a></td>";
                  echo "<td><input type=\"radio\" name=\"delid\" value=\"{attendanceid}\"></td>";
                  echo "<td> ".$row['attendanceid']."</td>
                  <td> ".$row['id']."</td>
                  <td> ".$row['first_name']."</td>
                  <td> ".$row['startsituation']."</td>
                  <td> ".$row['startyear']."年".$row['startmonth']."月".$row['startday']."日（".$row['startweek'].")"."</td>
                  <td> ".$row['field2']."</td>
                  <tr>
                  </tr>";

                }
                echo "
              </tbody>
            </table>
          </div>";
          $db = null;
        } catch (Exception $e) {
          echo $e->getMessage();
        }

        ?><br>

        <input type="submit" name="delete" value="削除">
      </form>
