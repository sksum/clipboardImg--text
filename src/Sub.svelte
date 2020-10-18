<script>
    import Img from './Img.svelte';
    import Text from './Text.svelte';
    document.onpaste = function(event){
		var items = (event.clipboardData || event.originalEvent.clipboardData).items;
		let index ;
		for (index in items) {
			var item = items[index];
			if (item.kind === 'file') {
				var blob = item.getAsFile();
				var reader = new FileReader();
				reader.onload = function(event){
                    src = event.target.result;
                    getText();
				};
				reader.readAsDataURL(blob);
			}
		}
    }

    import Tesseract from 'tesseract.js';
    let content =""
    const getText = () => {
        Tesseract.recognize(
        src,
        'eng',
        { logger: m => console.log(m) }
        ).then(({ data: { text } }) => {
        console.log(text);
        content= text;
        })
    }



    let src = "";
</script>

<table>
    <thead>
      <tr>
        <th>Image</th>
        <th>Text</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th><Img {src} /></th>
        <th><Text {content}/></th>
      </tr>
    </tbody>
  </table>
<style>
    table {
        position: absolute;
        width: 100%;
        height: 100%;
    }
</style>