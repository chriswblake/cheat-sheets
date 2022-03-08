
### Simple Example
```mermaid
  graph TD;
      A-->B;
      A-->C;
      B-->F;
      C-->D;
``` 

### Sequence Diagram  
```mermaid
sequenceDiagram
    participant dotcom
    participant iframe
    participant viewscreen
    dotcom->>iframe: loads html w/ iframe url
    iframe->>viewscreen: request template
    viewscreen->>iframe: html & javascript
    iframe->>dotcom: iframe ready
    dotcom->>iframe: set mermaid data on iframe
    iframe->>iframe: render mermaid
```

### Links in chart
```mermaid
flowchart LR;
    A-->B;
    B-->C;
    C-->D;
    click A callback "Tooltip for a callback"
    click B "http://www.github.com" "This is a tooltip for a link"
    click A call callback() "Tooltip for a callback"
    click B href "http://www.github.com" "This is a tooltip for a link"
```

### Pseudo-Fishbone
```mermaid
flowchart LR

    %% EXPERIENCES %
    subgraph Experience
        unable_to_connect
        slow_response
        invalid_results
        not_receiving_data
    end
    subgraph Issue
        not_starting --> unable_to_connect
        internet_loss --> unable_to_connect
        weak_connection --> slow_response
        not_paired --> unable_to_connect
        not_paired --> not_receiving_data
    end


    %% FAILURE AREA %%
    subgraph SIM
        deactivated --> internet_loss
        data_used_up --> internet_loss
        out_of_range --> weak_connection
    end
    subgraph Router 
        invalid_sim_config --> internet_loss
        https_enabled --> internet_loss
        invalid_dhcp_config --> internet_loss
        wrong_power_adapter --> not_starting
    end
    subgraph Computer
        turned_off --> not_starting & not_receiving_data
        overheated --> not_starting
    end
    subgraph Edge Management
        invalid_config --> not_starting
        invalid_config --> not_paired
        wrong_version --> not_paired
    end
    subgraph aurora
        invalid_config2 --> not_paired
        warn_sensor --> invalid_results
        no_power --> invalid_results
    end

```

### Gantt
```mermaid
gantt
dateFormat  YYYY-MM-DD
title Adding GANTT diagram to mermaid
excludes weekdays 2014-01-10

section A section
Completed task            :done,    des1, 2014-01-06,2014-01-08
Active task               :active,  des2, 2014-01-09, 3d
Future task               :         des3, after des2, 5d
Future task2              :         des4, after des3, 5d
```

### Expanding entries
```mermaid
sequenceDiagram
    participant Alice
    participant John
    links Alice: {"Dashboard": "https://dashboard.contoso.com/alice", "Wiki": "https://wiki.contoso.com/alice"}
    links John: {"Dashboard": "https://dashboard.contoso.com/john", "Wiki": "https://wiki.contoso.com/john"}
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
    Alice-)John: See you later!
```

## References
- Mermaid Syntax - https://mermaid-js.github.io/mermaid/
- GitHub Blog - Mermaid Support  - https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/