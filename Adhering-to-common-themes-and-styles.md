####Common themes and styles
When you change existing features or add new ones to the common themes and styles adhere existing styling of these files. Do not create a specific theme or style file that will be used only for your change.

1.	Use colors and fonts through our style libraries
    *	Use colors from colors.less (e.g. @neutralPrimaryColor)
    *	Use fonts from fonts.less (e.g. @regularFontFamily)
    *	See other less files in the StyleLibrary project for more collections of styles
1.	Update main CSS file instead of creating a specific one
If you need to add new styles, then do not create separate CSS file but add new style classes to the existing main CSS file (visuals.less)
