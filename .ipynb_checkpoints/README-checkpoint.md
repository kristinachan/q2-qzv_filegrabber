# q2-qzv_filegrabber
* view plot.png, grab raw-data.tsv or .csv, all within your interactive notebook! :)
* (saves you from having to download parent.qzv to local, drag to view.qiime2.org
* ...and then downloading data.tsv / screenshotting plot.png from the web interface)
* ...just to manually rename and reupload the data.tsv into your interactive notebook again.

for .tsv and .csv
* allows you to access the .qzv raw data as a dataframe variable
* allows you to save the data.tsv as parent_qzv_filename.tsv
* yay: now you won't so many generic raw-data.tsv files on your local...and you won't have to rename/organize them by hand!

for .png
* allows you to visualize the .png in your interactive notebook
* allows you to add a specific title for that .png\n
* tip: plt_title=f'your_title: beta_diversity_{filepathname_or_changingvarible}' allows you to retitle your plots according to which file it takes in.

this allows you to search through the .qzv zip file 
will return the first file that ends in with *nested_raw_data_file_str
* e.g. if there is only one raw-data.tsv in your .qzv and you're not sure if it's raw-data.tsv or raw_data.tsv 
* ... then nested_raw_data_file_str='data.tsv' will both find it for you

### to use:
download the qzv_filegrab.py into parent folder, e.g. python_scripts folder

>import sys\
>sys.path.append('/path/to/parent/folder/python_scripts/q2-qzv_filegrabber')\
>from qzvfilegrab import qzv filegrabber

### qzv_filegrabber(qzv_filepath_str, nested_raw_data_file_str='data.tsv', save=False, plt_title='optional_plt_title_for_png'):


    """
    Extracts a specific file from a .qzv archive and optionally saves it to the parent directory.
    
    Parameters:
    qzv_filepath_str (str): Path to the .qzv file.
    nested_raw_data_file_str (str): Relative path to the file within the .qzv archive. 
                                    Input: filename/end of the filename 
                                    '.tsv', '.csv', '.png', 
                                    'raw-data.tsv' or 'data.tsv'(for alpha/beta diversity),  
                                    'kruskal-wallis-pairwise-volume.csv' (for alpha diversity), 
                                    'permanova-pairwise.csv' (for beta diversity),
                                    'plot.png' (for lme), 'model_results.tsv' (for lme), etc..
                                    
                                    To find the name of the file type, you can:
                                    - go to view.qiime2.org drag and drop the file, and then inspect element 
                                    - unzip file in terminal to browse
                                    
    save (bool): If True, saves the extracted file to the parent directory of the .qzv file. Default is True.
    plt_title (str): Takes in a string if yo want to add a title to your .png. 
    
    
    Returns:
    str: Path to the saved file if `save` is True, 
         otherwise returns df if .csv or .tsv, 
         or displays if .png
    
    Example usage:
    raw_data_fp = qzv_filegrabber('/path/to/file.qzv', nested_raw_data_file_str='data.tsv', save=True)
    qzv_filegrabber('/path/to/file.qzv', nested_raw_data_file_str='plot.png', save=False, plt_title=f'optional_plt_title{changingvariable}_for_png')
    """
