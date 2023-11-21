# Plotting-ShotMap

# import-libraries
   
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    from matplotlib.colors import to_rgba
    import matplotlib.patheffects as path_effects
    from mplsoccer import Pitch, FontManager
    from highlight_text import ax_text
# Setup your DataFrame for Plotting
    combined_df = pd.read_excel('arsenalvburnleyshots.csv') #filelocation in brackets
    combined_df['markersize'] = combined_df['xg'] * 2000 #marker size for shot plotting according to xG
    df1 = combined_df.loc[combined_df['isHome']==True]
    df2 = combined_df.loc[combined_df['isHome']==False]
    totalxG1= df1['xg'].sum()
    totalxG2 = df2['xg'].sum()
    totalxG1 = format(totalxG1, '.2f')
    totalxG2 = format(totalxG2, '.2f')
    df2['x'] = 100 - df2['x']
    df2['y'] = 100 - df2['y']
    df2['xgoal'] = 100 - df2['xgoal']
    df2['ygoal'] = 100 - df2['ygoal']
    df1_missed = df1.loc[df1['shotType']=='miss']
    df2_missed = df2.loc[df2['shotType']=='miss']
    df1_saved = df1.loc[df1['shotType']=='save']
    df2_saved = df2.loc[df2['shotType']=='save']
    df1_goal = df1.loc[df1['shotType']=='goal']
    df2_goal = df2.loc[df2['shotType']=='goal']
    df2_goal
    
# Setup Pitch and Plot
    # Specify the URL or local path to the Oswald font file
    oswald_font_url = "https://raw.githubusercontent.com/google/fonts/main/ofl/oswald/Oswald%5Bwght%5D.ttf"
    oswald_regular = FontManager(oswald_font_url)

    pitch = Pitch(pitch_type='opta', pitch_color='white', line_alpha=0.5)
    fig, ax = pitch.draw(figsize=(12,10))

    # Plot the Shots Location

    pitch.scatter(df1_missed.x, df1_missed.y,color='red',s=df1_missed.markersize, ax=ax,alpha=0.5,edgecolors='#383838')

    pitch.scatter(df2_missed.x, df2_missed.y,color='purple',s=df2_missed.markersize, ax=ax,alpha=0.5,edgecolors='#383838')

    pitch.scatter(df1_saved.x, df1_saved.y,color='red',s=df1_saved.markersize,marker='h',alpha=0.75,ax=ax,edgecolors='black')

    pitch.scatter(df2_saved.x, df2_saved.y,color='purple',s=df2_saved.markersize,marker='h',alpha=0.75, ax=ax,edgecolors='black')

    pitch.scatter(df1_goal.x, df1_goal.y,color='red',s=df1_goal.markersize,marker='*', ax=ax,edgecolors='black')

    pitch.scatter(df2_goal.x, df2_goal.y,color='purple',s=df2_goal.markersize,marker='*', ax=ax,edgecolors='black')

     # Plot the Shoot Path

    pitch.lines(df1_goal.x,df1_goal.y,df1_goal.xgoal,df1_goal.ygoal,color='red',comet=True,ax=ax,alpha=0.35)
    pitch.lines(df2_goal.x,df2_goal.y,df2_goal.xgoal,df2_goal.ygoal,color='purple',comet=True,ax=ax,alpha=0.35)

    #Give Headings and Add Text to the Shot Map

    highlight_text = [{'color': 'red', 'fontproperties': oswald_regular.prop},
                  {'color': 'purple', 'fontproperties': oswald_regular.prop}]
    ax_text(50,90,"    <Arsenal> v. <Burnley> Shot Map", size=35, color='#000009',
                                fontproperties=oswald_regular.prop,highlight_textprops=highlight_text,
                                ha='center', va='center',ax=ax,weight='bold')
    ax.text(85,5,"@athalakbar13", size=20, color='#000009',
                                fontproperties=oswald_regular.prop,
                                ha='center', va='center',alpha=0.5)
    ax.text(20,82, f"(Total xG : {totalxG1})",size=20,color='red',alpha=0.85,
    fontproperties=oswald_regular.prop,
    ha='center', va='center',weight='bold')
    ax.text(80,82, f"(Total xG : {totalxG2})",size=20,color='purple',alpha=0.85,
    fontproperties=oswald_regular.prop,
    ha='center', va='center',weight='bold')


    plt.show()
