---
title: Using VLANs on Fortigate 30D
author: Maarten Van Driessen
type: post
date: 2016-02-26T08:36:28+00:00
url: /using-vlans-on-fortigate-30d/
categories:
  - Network
tags:
  - Fortigate
  - VLAN

---
While setting up a new Fortigate 30D for a client, I wanted to add a new VLAN for the guest Wi-Fi network. Usually, you just go into **Network** - **Interfaces** and add a new Interface there.

On the 30D however, this option wasn't there. After changing the device from switch mode to interface mode and back, I figured you can't do it in the GUI. The only way to do it on a 30D is by using the CLI.

To add a new VLAN, go to the dashboard, open up the CLI and enter these commands:

<div style="tab-size: 8" id="gist107922008" class="gist">
  <div class="gist-file" translate="no">
    <div class="gist-data">
      <div class="js-gist-file-update-container js-task-list-container file-box">
        <div id="file-fortigate-vlans" class="file my-2">
          <div itemprop="text" class="Box-body p-0 blob-wrapper data type-text  ">
            <div class="js-check-bidi js-blob-code-container blob-code-content">
              <p>
                <template class="js-file-alert-template">
              </p>
              
              <div data-view-component="true" class="flash flash-warn flash-full d-flex flex-items-center">
                <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert"> <path fill-rule="evenodd" d="M8.22 1.754a.25.25 0 00-.44 0L1.698 13.132a.25.25 0 00.22.368h12.164a.25.25 0 00.22-.368L8.22 1.754zm-1.763-.707c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0114.082 15H1.918a1.75 1.75 0 01-1.543-2.575L6.457 1.047zM9 11a1 1 0 11-2 0 1 1 0 012 0zm-.25-5.25a.75.75 0 00-1.5 0v2.5a.75.75 0 001.5 0v-2.5z"></path> </svg></p> 
                
                <p>
                  <span><br /> This file contains bidirectional Unicode text that may be interpreted or compiled differently than what appears below. To review, open the file in an editor that reveals hidden Unicode characters.<br /> <a href="https://github.co/hiddenchars" target="_blank">Learn more about bidirectional Unicode characters</a><br /> </span>
                </p>
                
                <div data-view-component="true" class="flash-action">
                  <a href="{{ revealButtonHref }}" data-view-component="true" class="btn-sm btn"> Show hidden characters<br /> </a>
                </div>
              </div>
              
              <p>
                </template><br /> <template class="js-line-alert-template"><br /> <span aria-label="This line has hidden Unicode characters" data-view-component="true" class="line-alert tooltipped tooltipped-e"><br /> <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-alert"> <path fill-rule="evenodd" d="M8.22 1.754a.25.25 0 00-.44 0L1.698 13.132a.25.25 0 00.22.368h12.164a.25.25 0 00.22-.368L8.22 1.754zm-1.763-.707c.659-1.234 2.427-1.234 3.086 0l6.082 11.378A1.75 1.75 0 0114.082 15H1.918a1.75 1.75 0 01-1.543-2.575L6.457 1.047zM9 11a1 1 0 11-2 0 1 1 0 012 0zm-.25-5.25a.75.75 0 00-1.5 0v2.5a.75.75 0 001.5 0v-2.5z"></path> </svg><br /> </span></template>
              </p>
              
              <table data-hpc class="highlight tab-size js-file-line-container js-code-nav-container js-tagsearch-file" data-tab-size="8" data-paste-markdown-skip data-tagsearch-lang="" data-tagsearch-path="Fortigate VLANs">
                <tr>
                  <td id="file-fortigate-vlans-L1" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="1">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC1" class="blob-code blob-code-inner js-file-line">
                    config system interface
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L2" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="2">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC2" class="blob-code blob-code-inner js-file-line">
                    edit GuestWifi
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L3" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="3">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC3" class="blob-code blob-code-inner js-file-line">
                    set type vlan
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L4" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="4">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC4" class="blob-code blob-code-inner js-file-line">
                    set vlanid 100
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L5" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="5">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC5" class="blob-code blob-code-inner js-file-line">
                    set interface vlan
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L6" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="6">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC6" class="blob-code blob-code-inner js-file-line">
                    set ip 172.16.100.1/24
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L7" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="7">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC7" class="blob-code blob-code-inner js-file-line">
                    next
                  </td>
                </tr>
                
                <tr>
                  <td id="file-fortigate-vlans-L8" class="blob-num js-line-number js-code-nav-line-number js-blob-rnum" data-line-number="8">
                  </td>
                  
                  <td id="file-fortigate-vlans-LC8" class="blob-code blob-code-inner js-file-line">
                    end
                  </td>
                </tr>
              </table>
            </div>
          </div></p>
        </div>
      </div>
    </div>
    
    <div class="gist-meta">
      <a href="https://gist.github.com/mvandriessen/108d19a02228793104159132b46d1194/raw/ecced9f6b4889aad96a1bff0a6a2419a9474a515/Fortigate%20VLANs" style="float:right">view raw</a><br /> <a href="https://gist.github.com/mvandriessen/108d19a02228793104159132b46d1194#file-fortigate-vlans"><br /> Fortigate VLANs<br /> </a><br /> hosted with &#10084; by <a href="https://github.com">GitHub</a>
    </div></p>
  </div>
</div>

After running these commands, you'll also see the VLAN show up in the **Network - Interfaces** page.