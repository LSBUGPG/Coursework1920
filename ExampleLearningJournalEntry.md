# Learning Journal

## 02/10/19

Investigate the data in the debug menu HUD. What do the min, max values mean? They appear to be the game update which runs asynchronously with the frame rate. If it is maxing at 33.3 ms it must be locked to 30 fps.

There is no way to get the Android build version from the `PlayerSettings` at runtime. However, you can get the version string which is cross platform. So I'm setting the version string to the Android build number for now.

The Xbox One Unity plugin does not call both `OnSignOut` callbacks if the player is offline.

The `uv`s in our models might be -0.5 to 0.5, 0 to 1, or a mixture.

Henry would like a visualisation to show the polygon density on screen.

## 03/10/19

Henry would like the tool to automatically tag models with out of bounds `uv`s. There are functions in `AssetDatabase` to get and set lists of labels. The results modify the meta files for each asset. Note that it tags all objects and sub-objects with the same tag, so embedded materials also get tagged.

`StandardHeightFog` shader wants to use the world position in its fragment shader, but on mobile, the symbol `UNITY_SHADER_SIMPLE` is defined which omits this data. I created a new structure which includes the height variable. I copied from the Unity shader source, so we'll need to upgrade it if we upgrade Unity.

This shader didn't work. The height variable was apparently ignored. I tried poking it into the `w` value of the transformed position – but this didn't compile (possibly due to a different problem). A machine reboot fixed the issue.

Instead I re-added the `worldPos` field and gave it `TEXCOORD 8`. This works, but might cause problems with using too many interpolators on lower Android devices. It works well for Quest though.

## 4/10/19

Laundrette, looking out into the street, runs at 45-47 FPS with reduced mist. With 10x mist particles, it runs at 41-43. Approximately ~4 FPS saving.

The flickering textures caused by polygons overlaid at the same z depth (z fighting.) Trying with a shader offset. One problem is that our shader stripper is stripping the new shader. I've added it to a shader variant list of whitelisted shaders but still no joy. Changing the colour output to verify if changes are going through at all. Definitely not using my shader!

The full process to get this working is: clear the variants in the Graphics settings, play the game in the editor, stop, save the variants to an asset, delete out the unwanted assets.

It works in a test program, but not in the game. The z resolution appears to be much lower on Quest. Increased `Offset` to 5, this appeared to work. We might need to iterate on this to find an ideal value. I also created a blended variation. Applying the solution to Victorian house level to see if this fixes the issue.

It looks like it might fix the issue, but causes others. It appears they have constructed their models in a bad way for layering the materials. And on the Quest, the z buffer can't cope and you get lots of z fighting. We need to come up with a standard way to handle this for all levels.

## 7/10/19

I've expanded my test program to display a wall and something over the top. If the objects are placed at the same z depth, then Offset -1,-1 seems to work at least out to 50 m. Why doesn't this work in the game?

Henry has now mesh baked the Victorian scene. The frame rate is better there now.

Problem merging svn versions. Attempting to merge a merge on a branch seems to cause a conflict with itself. The fix seems to be to merge with a block enabled on the merge itself, so that it ignores the merged revision. This should prevent further attempts to re-merge the problem files.

Problem: installing a python module for an older – no longer supported python version – into a new version of python. Seems you just need to find an older version of python. These older versions are no longer supported or updated, even for security fixes. Autodesk need to recompile against the newer versions of python. (They just can't be bothered.)
