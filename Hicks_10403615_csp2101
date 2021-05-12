#!/bin/bash
# Kyle Hicks 10403615

# a set of bool vars to control validation / continuation of serching
fname=false
searching=false
pvs=false
bvs=false
cset=false
efname=false
edname=false
efext=false

# harcoded values from the standard file structure
# video comment #1 - harcoding, associative array
values=( 'protocol' 'src_ip' 'src_port' 'dest_ip' 'dest_port' 'packets' 'bytes')

# FR4 - any normal class values auto excluded
# a concatenated string to eventually pass to the AWK block
# video comment #2 - hackish
awkstring='$13 !~/normal/ && '

# URER6 - until the user indicates to exit continue searching
until [ "$searching" = true ]; do

    # until a valid number of CRITERIA is provided continue prompting
    until [ "$cset" = true ]; do
        
        # FR1a - get CRITERIA COUNT 
        read -p "Please indicate how many criteria will be provided for the search [1], [2] or [3]): " criteria

        # if valid break out of the loop otherwise reprompt
        if [[ $criteria =~ ^[1-3] ]]; then
        
            cset=true

        else

            echo "Please enter a valid number test"
        
        fi
    done

    # create a list of CRITERIA VALUES for the user in a menu
    for (( i=0; i< ${#values[@]}; i++ )); do

        menu=$(( $i + 1 )) 
        echo "$menu: ${values[$i]}"

    done

    # for each of the CRITERIA COUNT selected get a CRITERIA VALUE
    # FR1b - validation on selections and other related data
    for (( i=1; i<=${criteria}; i++ )); do
        
        # a bool to control valid selections
        testa=false 

        # until a VALID CRITERIA provided prompt the user, otherwise get all CRITERIA
        until [ "$testa" = true ]; do

            # get the criteria
            read -p "Please enter no[$i] criteria: " crit
            
            # get the criteria value
            read -p "Please enter search value for [$crit] : " critv

                # if PROTOCOL            
                if [ "$crit" == 1 ]; then

                    # if valid selection set the bool to true
                    testa=true

                    # if the last CRITERIA then append to the string with break
                    # URE1a - appended in a manner which performs case insensitive searching
                    if [ $i == $criteria ]; then   

                        awkstring+='$3 ~/'$critv'/'

                    # else if not the last CRITERIA append to the spring with AND
                    else

                        awkstring+='$3 ~/'$critv'/ && '
                    
                    fi

                # if SRC_IP
                elif [ "$crit" == 2 ]; then

                    testa=true

                    # FR7a - returns partial searches in the AWK function
                    if [ $i == $criteria ]; then   

                        awkstring+='$4 ~/'$critv'/'

                    else

                        awkstring+='$4 ~/'$critv'/ && '
                    
                    fi

                # if SRC_PORT
                elif [ "$crit" == 3 ]; then

                    testa=true

                    if [ $i == $criteria ]; then   

                        awkstring+='$5 ~/'$critv'/'

                    else

                        awkstring+='$5 ~/'$critv'/ && '
                    
                    fi

                # if DEST_IP
                elif [ "$crit" == 4 ]; then

                    testa=true

                    if [ $i == $criteria ]; then   

                        awkstring+='$6 ~/'$critv'/'

                    else

                        awkstring+='$6 ~/'$critv'/ && '
                    
                    fi

                # if DEST_PORT
                elif [ "$crit" == 5 ]; then
                   
                    testa=true
                    
                    # FR7b - returns partial searches in the AWK function
                    if [ $i == $criteria ]; then   

                        awkstring+='$7 ~/'$critv'/'

                    else

                        awkstring+='$7 ~/'$critv'/ && '
                    
                    fi
                
                # if PACKETS
                elif [ "$crit" == 6 ]; then
                    
                    testa=true

                    # get sub searchable value from the user, continue prompting until valid value provided
                    # FR 5a - where packets get lt, gt etc
                    until [ "$pvs" = true ]; do
                        read -p "Enter [-gt] for greater than, [-lt] for less than, [-eq] for equal to or [!-eq] for not equal to: " pvar

                        if [ "$pvar" == "-gt" ]; then

                            pvarawk=">"
                            pvs=true
                        
                        elif [ "$pvar" == "-lt" ]; then
                        
                            pvarawk="<"
                            pvs=true

                        elif [ "$pvar" == "-eq" ]; then 
                        
                            pvarawk="=="                    
                            pvs=true

                        elif [ "$pvar" == "!-eq" ]; then
                            
                            pvarawk="!="     
                            pvs=true

                        else

                            echo "Please enter a valid value"
                        
                        fi
                    
                    done

                    # append to the string with the additional sub value
                    if [ $i == $criteria ]; then   

                        awkstring+='$8 '$pvarawk' '$critv' '

                    else

                        awkstring+='$8 '$pvarawk' '$critv' && '
                    
                    fi

                # if BYTES
                elif [ "$crit" == 7 ]; then

                    testa=true

                    # get sub searchable value from the user, continue prompting until valid value provided
                    # FR 5b - where packets get lt, gt etc
                    until [ "$bvs" = true ]; do
                        read -p "Enter [-gt] for greater than, [-lt] for less than, [-eq] for equal to or [!-eq] for not equal to: " bvar

                        if [ "$bvar" == "-gt" ]; then

                            bvarawk=">"
                            bvs=true
                        
                        elif [ "$bvar" == "-lt" ]; then
                        
                            bvarawk="<"
                            bvs=true

                        elif [ "$bvar" == "-eq" ]; then 
                        
                            bvarawk="=="                    
                            bvs=true

                        elif [ "$bvar" == "!-eq" ]; then
                            
                            bvarawk="!="     
                            bvs=true

                        else

                            echo "Please enter a valid value"
                        
                        fi
                    done

                        # append to the string with the additional sub value
                        if [ $i == $criteria ]; then   

                            awkstring+='$9 '$bvarawk' '$critv' '

                        else

                            awkstring+='$9 '$bvarawk' '$critv' &&  '
                        fi

                else

                    echo "Please enter a valid selection from the list"
               
                fi

            done                
     
    done
  
    # until a valid number is provided continue prompting
    until [ "$fname" = true ]; do
        
        echo "Following files are available for selection:"
        ls | grep "serv_acc_log_" | cat -n

        # FR2 - get a SERVER LOG NAME from user
        # Video Comment 3 - poor menu option requiring full filename
        read -p "Please enter the file name to search or hit 0 for all: " fsearch
        
        # URER1b - filename search case insensitive
        fsearch=$(echo "$fsearch" | tr '[:upper:]' '[:lower:]')

        # if FILE EXISTS and FILE MATCHES pattern for server log files
        if [ -e "$fsearch" ] && [[ $fsearch =~ "serv_acc_log_" ]]; then

            # set the file name as being valid
            fname=true
                    
        elif [ $fsearch = 0 ]; then

            fname=true

            # set all files names with pattern as the
            fsearch="serv_acc_log_*.csv"
        
        else

            echo "file doesnt exist"
        fi
        
    done
    
    
    # ENHANCED FUNCTIONALITY - export to csv or txt
    until [ "$efext" = true ]; do

        read -p "Enter 'csv' or 'txt' for export file type: " exporttype
        
        if [[ "$exporttype" =~ "csv"|"txt" ]]; then

            exporttype="."$exporttype
            efext=true

        else

            echo "Not a validate file extension type"

        fi
    
    done

    # FR3a - Get a file name from the user
    # until a valid file name is provided continue prompting 
    until [ "$efname" = true ]; do

        read -p "Enter the name of file where the results will be exported to: " exportfilename

        if [ -z "$exportfilename" ]; then
                
            echo "File name cannot be blank"
                
        elif [[ $exportfilename =~ [^A-Za-z0-9] ]]; then
                
            echo "File name cannot contain special characters"

        else 
                
            efname=true
            exportfilename+=$exporttype
            
        fi
   
    done

    # FR3b - Get a dir name from the user
    # until a valid dir name is provided continue prompting
    until [ "$edname" = true ]; do
        
        read -p "Enter the name of the destination directory for the file: " exportdirname

        if [ -z "$exportdirname" ]; then

            echo "Directory name cannot be blank"
                        
        else 

            edname=true
               
        fi

    done
    
    # FR3b - Create the directory if doesnt already exist
    if [[ ! -e "$exportdirname" ]]; then
    
        mkdir $exportdirname

    fi

    # FR3c - Export the results to the file and directory names provided by the user
    # FR6 - In the END block totals for bytes and/or packets are displayed where selected
    # URER2 - results printed in columnar format
    # Video Comment 4 - NR>1
    
    awk -v pvs=$pvs -v bvs=$bvs 'BEGIN {FS=","; IGNORECASE=1;}
                '"$awkstring"' {
                if ("'$exporttype'" == ".txt")
                    {                
                     
                        printf " %-15s %-15s %-15s %-20s %-10s %-20s %-10s %-20s %-10s %-10s %-10s %-10s %-10s \n",  $1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11, $12, $13 >"'$exportfilename'";
                        printf " %-15s %-15s %-15s %-20s %-10s %-20s %-10s %-20s %-10s %-10s %-10s %-10s %-10s \n",  $1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11, $12, $13 ;
                        sump+=$8;
                        sumb+=$9;
                    }                    
                
                else if ("'$exporttype'" == ".csv")
                    {
                        
                        print $1","$2","$3","$4","$5","$6","$7","$8","$9","$10","$11","$12","$13 >"'$exportfilename'";
                        printf " %-15s %-15s %-15s %-20s %-10s %-20s %-10s %-20s %-10s %-10s %-10s %-10s %-10s \n",  $1, $2, $3, $4, $5, $6, $7, $8, $9, $10, $11, $12, $13 ;
                        sump+=$8;
                        sumb+=$9;
                    }
                }
        END {  
        if ( pvs == "true" && sump != 0 )
            {
                printf " %-111s %-80s \n", "Total Packets:", sump ;
            }
        
        if ( bvs == "true" && sumb != 0 )
            {
                printf " %-132s %-10s \n", "Total Bytes:", sumb ;
            }

        }' $fsearch

    # if no results let user know otherwise move the file
    if [[ ! -f "$exportfilename" ]]; then
                
        echo "No results found"
     
    else 

        mv $exportfilename $exportdirname
    
    fi

# Either repeat the search or exit     
read -p "Enter any key to repeat process or 0 to exit " exot

    if [ "$exot" == "0" ]; then
        searching=true
    
    # reset any bool variables when new searching
    else
        cset=false
        critarr=()
        critval=()
        awkstring='$13 !~/normal/ && '
        bvs=false
        pvs=false
        fname=false
        efname=false
        edname=false
        efext=false
        eftype=false
    fi

done

exit 0