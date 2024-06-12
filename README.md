# q2-qzv_filegrabber
* view plot.png, grab raw-data.tsv or .csv, all within your interactive notebook! :)
* implementation can allow you to pull statistics from multiple qzvs, combine in one dataframe, and sort by pvalue to quickly explore data
* also saves you from having to download parent.qzv to local, drag to view.qiime2.org
* saves time by eliminating requirement to manually downloading data.tsv / screenshot plot.png from the web interface
* file naming by variables can be implemented to reduce human error 

for .tsv and .csv
* allows you to access the .qzv raw data as a dataframe variable
* allows you to save the data.tsv as parent_qzv_filename.tsv
* downloaded raw-data.tsv files on your local will not be redundant, take memory, or need to be renamed by hand

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

Example usage to querey all stats into one dataframe:

    """
    from qiime2.plugins.diversity.visualizers import beta_group_significance

    distance_matrix = {
        'uwuf': core_metrics_subexp_subset.unweighted_unifrac_distance_matrix,
        'wuf': core_metrics_subexp_subset.weighted_unifrac_distance_matrix,
        'jcd': core_metrics_subexp_subset.jaccard_distance_matrix,
        'bc': core_metrics_subexp_subset.bray_curtis_distance_matrix
    }

    analysis_columns = ['mouse_genotype_treatment'] #, 'mouse_genotype', 'mouse_treatment', 'mouse_age']
    stats_summary_df_list = []

    for key, distance_matrix in distance_matrix.items():
        for analysis in analysis_columns:

            permdisp = beta_group_significance(distance_matrix, metadata.get_column(f'{analysis}'), method='permdisp', pairwise=True)
            permdisp_fp = f'{sampletype_dir}{core_metrics_dir}/permdisp_{key}_{preptype_label}_{subexp}_{subset}.qzv'
            permdisp.visualization.save(permdisp_fp)
            permdisp_summary_df = qzv_filegrabber(permdisp_fp, 'permdisp-pairwise.csv')
            permdisp_summary_df['beta_method'] = key
            permdisp_summary_df['stat_method'] = 'permdisp'
            stats_summary_df_list.append(permdisp_summary_df)

            permanova = beta_group_significance(distance_matrix, metadata.get_column(f'{analysis}'), method='permanova', pairwise=True)
            permanova_fp = f'{sampletype_dir}{core_metrics_dir}/permanova_{key}_{preptype_label}_{subexp}_{subset}.qzv'
            permanova.visualization.save(permanova_fp)   
            permanova_summary_df = qzv_filegrabber(permanova_fp, 'permanova-pairwise.csv')
            permanova_summary_df['beta_method'] = key
            permanova_summary_df['stat_method'] = 'permanova'
            stats_summary_df_list.append(permanova_summary_df)

            anosim = beta_group_significance(distance_matrix, metadata.get_column(f'{analysis}'), method='anosim', pairwise=True)
            anosim_fp = f'{sampletype_dir}{core_metrics_dir}/anosim_{key}_{key}_{preptype_label}_{subexp}_{subset}.qzv'
            anosim.visualization.save(anosim_fp)
            anosim_summary_df = qzv_filegrabber(anosim_fp, 'anosim-pairwise.csv')
            anosim_summary_df['beta_method'] = key
            anosim_summary_df['stat_method'] = 'anosim'
            stats_summary_df_list.append(anosim_summary_df)

    combined_df = pd.concat(stats_summary_df_list, ignore_index=True)
    combined_df = combined_df.sort_values(by='q-value', ascending=True)
    """ 