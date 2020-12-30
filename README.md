# driverless HoloPlay.js
Patch for HoloPlay.js v0.2.3 to support (mobile) devices without HoloPlay service.

This patch:
1) Removes the need for HoloPlay service (via hardcoded calibration values)
2) Brings HoloPlay.js to webkit browsers (Safari), incl. on iPadOS for the first time
3) Slims the codebase down by roughly 30%

Due to copyright reasons I am not distributing the patched HoloPlay.js directly.

Minimal Python example that creates the patched library for you:

```python
import urllib.request
from diff_match_patch import diff_match_patch

with urllib.request.urlopen('https://cdn.jsdelivr.net/npm/holoplay@0.2.3/holoplay.js') as f:
	holoplay_js_vanilla = f.read().decode('utf-8')

with urllib.request.urlopen('https://raw.githubusercontent.com/jankais3r/driverless-HoloPlay.js/main/holoplay.js.patch') as f:
	diff = f.read().decode('utf-8').replace('\r\n', '\n')
	dmp = diff_match_patch()
	patches = dmp.patch_fromText(diff)
	holoplay_js, _ = dmp.patch_apply(patches, holoplay_js_vanilla)
	holoplay_js = holoplay_js.replace(
	# Original calibration:
	'{"configVersion":"1.0","serial":"00000","pitch":{"value":49.825218200683597},"slope":{"value":5.2160325050354},"center":{"value":-0.23396748304367066},"viewCone":{"value":40.0},"invView":{"value":1.0},"verticalAngle":{"value":0.0},"DPI":{"value":338.0},"screenW":{"value":2560.0},"screenH":{"value":1600.0},"flipImageX":{"value":0.0},"flipImageY":{"value":0.0},"flipSubp":{"value":0.0}}',
	# Your calibration:
	'{"configVersion":"1.0","serial":"00000","pitch":{"value":47.56401443481445},"slope":{"value":-5.480000019073486},"center":{"value":0.374184787273407},"viewCone":{"value":40.0},"invView":{"value":1.0},"verticalAngle":{"value":0.0},"DPI":{"value":338.0},"screenW":{"value":2560.0},"screenH":{"value":1600.0},"flipImageX":{"value":0.0},"flipImageY":{"value":0.0},"flipSubp":{"value":0.0}}')

```
After this, the `holoplay_js` variable holds the patched code that can be injected into the hologram scene served by your server. To see the code in action, check out my other repo [HoloSkype](https://github.com/jankais3r/HoloSkype).

To see how this can be used on an iPad, check out the [iOS-LookingGlass](https://github.com/jankais3r/iOS-LookingGlass) repo.
