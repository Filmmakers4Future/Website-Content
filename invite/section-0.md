HiddenFromArchive: true
HiddenFromAuthor: true
HiddenFromCategory: true
HiddenFromSearch: true
HiddenFromSitemap: true
Section: true
SectionBackground: alternate
NoMarkdown: true

<script>
window.onload = function () {
	// show invite tool - only shows when javascript is enabled
	document.getElementById('invite_tool').style["display"] = "";
		
	$('#mail').tagThis({
		noDuplicates: true,
		email : true,
		defaultText: "Email address"
	});
}

function copyToClipboard(val){
		element = $('<textarea>').appendTo('body').val(val).select()
		document.execCommand('copy')
		element.remove()
}

function processData(action){
		var cc = ""
		var addresses = $('#mail').data('tags');
		if (addresses){
			for (address of addresses) {
				cc +=address["text"]+","
			}
		}
		var body = $('#invite_body').val();
		if (action == "openMail") {
			body = body.replace(/\r\n|\r|\n/g, "%0D%0A");
			location.href = "mailto:?bcc=" + cc + "&subject=Invite to Filmmakers for Future&body=" + body
		} else if (action == "copy") {
			copyToClipboard("Please copy the list of email addresses into the bcc field of your mail service and then remove it from the mail:\r\n\r\n" + cc + "\r\n\r\n" + body)
		}
	};
</script>

<div class="row justify-content-center" id="invite_tool" style="display:none">
	<div class="col-lg-9 text-center text-white">
		<!-- Email -->
		<div class="mb-1 small text-white-75">You can enter multiple email addresses - just press Enter or add a comma after each one.</div>
		<input type="email" id="mail" maxlength="256"class="form-control mb-4">
		<!-- Message -->
		<div class="form-group">
<textarea id="invite_body" rows="10" class="form-control mb-4 mt-4">
Hi,
I have signed the public statement by Filmmakers for Future and would like to encourage you to sign as well!

---- Statement ----
Fiction and visual storytelling in general have the ability to construct realities, which is why we as filmmakers have a special responsibility to society. This applies not only to the stories we tell, but also to the way our industry works.
--------- Continue reading and sign here: https://filmmakersforfuture.org

---- The main goals of Filmmakers for Future can easily be summarized ----
1. Minimizing the environmental impact of the global film industry
2. Sustainable storytelling, i.e. what ideals do we convey and what stories do we tell
3. Raising awareness of the crew and the public
4. Socially responsible working conditions and a diverse industry
---------

You can also join their working groups to discuss department-specific topics and participate in making the industry fit for the future. Help is very much welcome! Visit https://filmmakersforfuture.org/participate/ for more information.

Best,
</textarea>
		</div>
	</div>
	
	<div class="col-lg-4 text-center text-white">
		<button class="btn btn-info btn-block mb-1" onclick="processData('openMail')">Open in default email program</button>
	</div>
	<div class="col-lg-4 text-center text-white">
		<button class="btn btn-info btn-block mb-1" onclick="processData('copy')">Copy to the clipboard</button>
	</div>
</div>

<noscript>
	<div class="text-center text-white pt-5 pb-5">
	 <b>To use the invite tool it is necessary to enable JavaScript.<br>
	 You can follow <a href="https://www.enable-javascript.com/">the instructions</a> to enable JavaScript in your web browser.</b>
	</div>
</noscript>