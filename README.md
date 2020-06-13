# bash_history_search
Search bash history with [n] keywords
Searching history for all the keywords in any order, add this function into .bashrc file
function hg(){ 
    if [[ $# -eq 0 ]]; then
        echo -e "hg <keyword1> <keyword2> <keyword3> ... etc"
        echo -e "\033[36mSearching history for all the keywords in \033[32many order\033[0m"
    else # history | grep -Pi "^(?=.*it24)(?=.*it22)(?=.*log).*$"
        grepString="[ ]+[0-9]+[ ]+.*"; [[ -f /tmp/debug_hg ]] && echoG "grepString = $grepString"
        for (( i=1 ; i <= "$#"; i=i+1 )); do
            paramArray=( "${@:$i}" )                
            thisParam="$(sed 's/\./\\\./g; s/\-/\\\-/g; s/\+/\\+/g; s/\*/\\\*/g; s/\?/\\\?/g'<<< "${paramArray[0]}")"
            [[ $i -eq 1 ]] && HI_CMD="($thisParam)" || HI_CMD="$HI_CMD|($thisParam)"
            grepString="${grepString}(?=.*$thisParam)"; [[ -f /tmp/debug_hg ]] && echoG "grepString = $grepString"
        done
        [[ -f /tmp/debug_hg ]] && echoG "HI_CMD = $HI_CMD"
        history | grep -Pi "$grepString" | hilightGi "$HI_CMD"
    fi
}
