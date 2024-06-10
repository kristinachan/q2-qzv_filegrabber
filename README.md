# q2-qzv_filegrabber
view plot.png, grab raw-data.tsv, within your interactive notebook.

### to use:
download the qzv_filegrab.py into parent folder, e.g. python_scripts folder\

import sys\
sys.path.append('/path/to/parent/folder/python_scripts')\
from qzv_filegrab import qzv filegrabber

#### def qzv_filegrabber(qzv_filepath_str, nested_raw_data_file_str='data/raw-data.tsv', save=False):

 """
    Extracts a specific file from a .qzv archive and optionally saves it to the parent directory.
    
    Parameters:
    qzv_filepath_str (str): Path to the .qzv file.
    nested_raw_data_file_str (str): Relative path to the file within the .qzv archive. 
                                    Can be: 'raw-data.tsv' (for alpha/beta diversity),  
                                    'kruskal-wallis-pairwise-volume.csv' (for alpha diversity), 
                                    'permanova-pairwise.csv' (for beta diversity),
                                    'plot.png' (for lme), 'model_results.tsv' (for lme), etc..
                                    
                                    To find the name of the file type, you can:
                                    - go to view.qiime2.org drag and drop the file, and then inspect element 
                                    - unzip file in terminal to browse
                                    
    save (bool): If True, saves the extracted file to the parent directory of the .qzv file. Default is True.
    
    Returns:
    str: Path to the saved file if `save` is True, otherwise None.
    
    Example usage:
    raw_data_fp = qzv_filegrabber('/path/to/file.qzv', nested_raw_data_file_str='raw-data.tsv', save=True)
    qzv_filegrabber('/path/to/file.qzv', nested_raw_data_file_str='plot.png', save=False)
    """
