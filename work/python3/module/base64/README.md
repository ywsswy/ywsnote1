
base64.standard_b64encode("/".encode("utf-8")).decode("utf-8")
'Lw=='

base64.standard_b64decode("Lw==").decode("utf-8")
'/'
