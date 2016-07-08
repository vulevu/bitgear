# bitgear
Project for interview for BitGear
<?php
namespace PhoneNumberSearch;
class PhoneNumeberSearch {
	
	private $path;
	
	public function __construct($path) {
		if($path != null && file_exists($path)) {
			$this->path = $path;
		}	
	}
	
	public function matchMSISDN($text) {
		preg_match_all('/\+[\d]{12}/', $text, $matches);
		return $matches[0];
	}
	
	public function search() {
		//echo 'Start search on '.$this->path."\n";
		$result = [];
		if(empty($this->path)) {
			return $result;
		}
		$directory = new \RecursiveDirectoryIterator($this->path);
		$iterator = new \RecursiveIteratorIterator($directory);
		
		foreach ($iterator as $item) {
			$current_file = $item->getPathname();
			//echo "Checking $current_file \n";
			if (!(is_file($current_file) && mime_content_type($current_file) == 'text/plain')) {
				continue;
			}
			//echo "$current_file is text file \n";
			$matches = $this->matchMSISDN(file_get_contents($current_file));
			//var_dump($matches);
			if (!empty($matches)) {
				array_push($result, ['path'=>$current_file, 'matches'=>$matches]);
			}
		}
		
		$output = print_r($result, true);
		file_put_contents('file.txt', $output);
	}
	
}

/*Primer:*/
$pns = new PhoneNumeberSearch('.');
$res = $pns->search();
var_dump($res);


?>
