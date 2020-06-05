HiddenFromArchive: true
HiddenFromAuthor: true
HiddenFromCategory: true
HiddenFromSearch: true
HiddenFromSitemap: true
Section: true
NoMarkdown: true

<script>
window.onload = function () {
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
		var body = $('#body').val();
		if (action == "openMail") {
			body = body.replace(/\r\n|\r|\n/g, "%0D%0A");
			location.href = "mailto:?bcc=" + cc + "&subject=Invite to Filmmakers for Future&body=" + body
		} else if (action == "copy") {
			copyToClipboard("Please copy the list of email addresses into the bcc field of your mail service and then remove it from the mail:\r\n\r\n" + cc + "\r\n\r\n" + body)
		}
	};
</script>

<div class="row justify-content-center">
	<div class="col-lg-9 text-center text-white">
		<!-- Email -->
		You can enter multiple email addresses - just press Enter or add a comma after each one.
		<input type="email" id="mail" maxlength="256"class="form-control mb-4">
		<!-- Message -->
		<div class="form-group">
<textarea id="body" rows="10" class="form-control mb-4">
Hi,
I have signed the public statement by Filmmakers for Future and would like to encourage you to sign as well!

---- Statement ----
Fiction and visual storytelling in general have the ability to construct realities, which is why we as filmmakers have a special responsibility to society. This applies not only to the stories we tell, but also to the way our industry works.
--------- Continue reading and sign here: https://filmmakersforfuture.org.

---- The main goals of Filmmakers for Future are simply summed up in three points ----
1. Minimizing the environmental impact of our industry while maintaining socially acceptable working conditions
2. Sustainable storytelling, i.e. what ideals do we convey and what stories do we tell
3. Raising awareness of the crew and the public
---------

You can also join their working groups and participate in making the industry fit for the future. Help is very much welcome! Visit https://filmmakersforfuture.org/participate/ for more information.

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
	<div class="col-lg-10 text-center text-white mt-4">
		<span class="text-white-75 text-center">
			<strong>Privacy Information</strong><br>The information you enter in this form will not be sent to our server.<br>Instead, a message with the information you entered will open in your default email program.<br>There you can add more contacts and then send the email.
		</span>
	</div>
</div>