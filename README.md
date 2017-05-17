<?php
error_reporting(0);

$testa = $_POST['veio'];
$nome  = $_GET['nome'];

if($testa != "") {
	require_once('class.phpmailer.php');
    
	$de 	  = $_POST['de'];
    $to       = $_POST['emails'];  
    $subject  = $_POST['assunto'];
    $anexo_nome  = trim($_POST['textok']); 
    $email    = explode("\n", trim($to));
    
	function generateRandomHexa($length = 10) {
		$characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
		$charactersLength = strlen($characters);
		$randomString = '';
		for ($i = 0; $i < $length; $i++) {
			$randomString .= $characters[rand(0, $charactersLength - 1)];
		}
		return $randomString;
	}
	
	function generateRandomString($length = 10) {
		$characters = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
		$charactersLength = strlen($characters);
		$randomString = '';
		for ($i = 0; $i < $length; $i++) {
			$randomString .= $characters[rand(0, $charactersLength - 1)];
		}
		return $randomString;
	}
	
	function generateRandomInteger($length = 10) {
		$characters = '0123456789';
		$charactersLength = strlen($characters);
		$randomString = '';
		for ($i = 0; $i < $length; $i++) {
			$randomString .= $characters[rand(0, $charactersLength - 1)];
		}
		return $randomString;
	}		
	
	function generateRandomZero($length = 6) {
		$characters = '0';
		$charactersLength = strlen($characters);
		$randomString = '';
		for ($i = 0; $i < $length; $i++) {
			$randomString .= $characters[rand(0, $charactersLength - 1)];
		}
		return $randomString;
	}				
	
       function random_num(){
		for($x = 0; $x < 5; $x++){
			$n = $n . rand(1,9);
		}
		return generateRandomZero(rand(0,6)) . $n;
       }
    
       function random_subject(){
		for($x = 0; $x < 5; $x++){
			$n = $n . rand(1,9);
		}
		return mt_rand(1,9) . $n;
       }		
	
       function random_alt(){
		$rn = rand(4,8);
		for($x = 0; $x < $rn; $x++){
			$n = $n . rand(1,9);
		}
		return mt_rand(1,90) . '-' . $n;
       }				
    
	$mail = new PHPMailer();						
	$mail->IsMail();
	$mail->CharSet 	= 'UTF-8'; // Charset da mensagem (opcional)		
	$mail->IsHTML(true); // Configura um e-mail em HTML
	
	//loop
       $i = 0;
       $count = 1;		
	
	while($email[$i]) {
	
	    
		
$do= $_SERVER['HTTP_HOST'];		
		
$arrX = array("Oliver", "Jack", "Harry", "Jacob", "Charlie", "Thomas", "George", "Oscar", "James", "William", "Noah", "Alfie", "Joshua", "Muhammad", "Henry", "Leo", "Archie", "Ethan", "Joseph", "Freddie", "Samuel", "Alexander", "Logan", "Daniel", "Isaac", "Max", "Mohammed", "Benjamin", "Mason", "Lucas", "Edward", "Harrison", "Jake", "Dylan", "Riley", "Finley", "Theo", "Isis", "Sebastian", "Adam", "Zachary", "Arthur", "Toby", "Luke", "Harley", "Lewis", "Tyler", "Harvey", "Matthew", "David", "Reuben", "Michael", "Elijah", "Kian", "Tommy", "Mohammad", "Blake", "Luca", "Theodore", "Stanley", "Nathan", "Charles", "Frankie", "Jude", "Teddy", "Louie", "Louis", "Ryan", "Hugo", "Bobby", "Dexter", "Ollie", "Alex", "Liam", "Kai", "Gabriel", "Connor", "Aaron", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley", "Stanley" );
$randIndex = array_rand($arrX);
		

$remetentek = $arrX[$randIndex]."@".$do;
	
		
		
		$mail->ClearAllRecipients();
		
		$ok = "ok";
		$aux = explode(';', $email[$i]);
		
		if ($de == ''){
			$de = "$nome".generateRandomInteger(2) ."@". gethostname();
		}			
		
		$de = str_replace("%random_num%", random_num(), $de);
		$de = str_replace("%random_subject%", random_subject(), $de);		
		
		$nome = str_replace("%random_num%", random_num(), $nome);
		$nome = str_replace("%random_subject%", random_subject(), $nome);
		
		$subject = str_replace("%random_num%", random_num(), $subject);
		$subject = str_replace("%random_alt%", random_alt(), $subject);		
		$subject = str_replace("%random_subject%", random_subject(), $subject);
		
		$mensagem = $_POST['html'];
		
		$pos = strpos($mensagem, "<body>");
		if ($pos === false) {
			$tmp       = $mensagem;
			$mensagem  = "<body>\n";
			$mensagem .= $tmp . "\n";
			$mensagem .= "</body>";
		}			
		
		$pos = strpos($mensagem, "<html>");
		if ($pos === false) {
			$tmp       = $mensagem;
			$mensagem  = "<html>\n";
			$mensagem .= $tmp . "\n";
			$mensagem .= "</html>";
		}
		
		$mensagem = str_replace("%random_num%", random_num(), $mensagem);
		$mensagem = str_replace("%random_alt%", random_alt(), $mensagem);		
		$mensagem = str_replace("%random_subject%", random_subject(), $mensagem);						
		$mensagem = str_replace("%generateRandomHexa%", generateRandomHexa(rand(5, 10)), $mensagem);
		$mensagem = str_replace("%generateRandomString%", generateRandomString(rand(5, 30)), $mensagem);		
		$mensagem = str_replace("%generateRandomInteger%", generateRandomInteger(rand(5, 20)), $mensagem);		
		$mensagem = str_replace("</html>", '<br><br><br><br><br><br><br><font color="#E6E6E6">n_'.generateRandomInteger(rand(20, 99)).'</font></html>', $mensagem);
		
		$mail->From 	= $remetentek;
		$mail->FromName = $nome;
		$mail->Subject  = $subject;
		$mail->Body 	= $mensagem;
		$mail->AltBody 	= trim(strip_tags($mensagem));
		
		//$mail->AddAddress($email[$i]);
		$mail->AddAddress($aux[0]);
        $mail->AddAttachment($anexo_nome);		
		
		if($mail->Send()){
               echo "$count <font color=Black>SEND MAIL ".$aux[0]."</font><br><hr>";
		}else{
               echo "$count <font color=red>ERRO</font>".$mail->ErrorInfo."<br><hr>";				
		}
		
		$i++;
		$count++;			
	}

	
	$count--;

	if($ok == "ok")
	echo "</font>"; 
	echo "<font color=blue>[Fim do Envio]</font><br>";
		
} 
?>


<style type="text/css">
body {
	background-image: url();
	background-repeat: no-repeat;
	background-color: #F9F9F9;
}
p {
	font-weight: bold;
}
body,td,th {
	font-family: Tahoma, Geneva, sans-serif;
	font-size: 14px;
	color: #000;
}
.form {	font-family: Arial, Helvetica, sans-serif;
	font-size: 10px;
	color: #333333;
	background-color: #FFFFFF;
	border: 1px dashed #666666;
}
.texto {	font-family: Verdana, Arial, Helvetica, sans-serif;
	font-weight: bold;
}
</style>
<title>SEND PINK</title>
<form action="?nome=<?php echo $nome; ?>" method="post" enctype="multipart/form-data" name="form1">
  <input type="hidden" name="veio" value="sim"></head>

<body>
<p>&nbsp;</p>
<p>Nome/Remetente:
  <input name="nome" value="<?php echo $nome; ?>" type="text" class="form" id="nome" style="width:20%" >
  /<input name="de" value="" type="text" class="form" id="de" style="width:20%" >
</p>
<p>Assunto:********* 
<input name="assunto" type="text" value="" class="form" id="assunto" style="width:40%" ></p>
<p>Nome anexo:********* 
<input name="textok" type="text" class="form" id="textok" style="width:40%" value="Ordem_0009732823.html" >
</p>
<p class="texto">AQUI PRECISA DE SENHA BONITINHO</p>
<p class="texto">
  <input name="senha" value="" type="text" class="form" id="senha" style="width:49%" >
</p>
<p>&nbsp;</p>
<p>
<input type="submit" name="Submit" id="Submit" value="Enviar >>"></p>
<p><textarea name="html" style="width:25%" rows="10" wrap="VIRTUAL" class="form" id="html"></textarea>
  *.*
<textarea name="emails" style="width:25%" rows="10" wrap="VIRTUAL" class="form" id="emails"></textarea></p>
</body>
</html>
