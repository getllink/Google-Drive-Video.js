# Google-Drive-Video.js
Google Drive Video.js Player Script

The script using [api.getlinkdrive.com](http://api.getlinkdrive.com) to grab google drive streaming links

	<?php
	$link = 'https://drive.google.com/file/d/0B6VYU2mjTdy0WVRjb1BJUU1hYXM/view';
	$api = 'http://api.getlinkdrive.com/getlink?url='.$link;
	$sources = curl($api);
	function curl($url)
	{
		$ch = @curl_init();
		curl_setopt($ch, CURLOPT_URL, $url);
		$head[] = "Connection: keep-alive";
		$head[] = "Keep-Alive: 300";
		$head[] = "Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7";
		$head[] = "Accept-Language: en-us,en;q=0.5";
		curl_setopt($ch, CURLOPT_USERAGENT, 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.124 Safari/537.36');
		curl_setopt($ch, CURLOPT_ENCODING, 'gzip');
		curl_setopt($ch, CURLOPT_HTTPHEADER, $head);
		curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
		curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE);
		curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
		curl_setopt($ch, CURLOPT_TIMEOUT, 60);
		curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 60);
		curl_setopt($ch, CURLOPT_FOLLOWLOCATION, TRUE);
		$page = curl_exec($ch);
		curl_close($ch);
		return $page;
	}
	?>
	<script type="text/javascript" src="http://api.getlinkdrive.com/js/videojs/video.js"></script>
	<script type="text/javascript" src="http://api.getlinkdrive.com/js/videojs/videojs-resolution-switcher.js"></script>
	<link href="http://api.getlinkdrive.com/js/videojs/video-js.css" rel="stylesheet">
	<link href="http://api.getlinkdrive.com/js/videojs/videojs-resolution-switcher.css" rel="stylesheet">
	<div id="player" style="width: 100%;height: 100%">
	<video id="video" class="video-js vjs-default-skin"></video>
	</div>
	<script type="text/javascript">
	videojs('video', {
	    width: 'auto',
	    height: 'auto',
	    controls: true,
	    plugins: {
	      videoJsResolutionSwitcher: {
	        default: 480,
	        dynamicLabel: true
	      }
	    }
	}, function(){
	    var player = this;
	    window.player = player
	    player.updateSrc(<?php echo $sources; ?>)
	    player.on('resolutionchange', function(){
	      	console.info('Source changed to %s', player.src())
	    })
	});
	</script>

More info: [GetLinkDrive](http://getlinkdrive.com/)
