![MSUS Solution Accelerator](./images/MSUS%20Solution%20Accelerator%20Banner%20Two_981.png)

# Manufacturing Vision Acceleratoer ARM64v8
Welcome to the Manufacuring Vision Accelerator repo for ARM64v8 Nvidia devices, i.e. Jetson Nano, Jetson NX, Jetson Xavier. We hope what you find here may be helpful to your pursuit of vision analytics on the Edge! The ARM64v8 version of the Accelerator was created by myself and Rick Durham (Azure AI/ML), using the baseline code developed in conjunction with Nick Kwiecien, Ph.D. 

This is a sample repository of working code - nothing more, nothing less.  There are no warranties or guarantees stated, or otherwise implied.


[Architecture Walkthrough Video](/video/architectural_overview.mp4)


[Demo Walkthrough Video](/video/Demo_Walkthrough.mp4)


[Accelerator Overview Video](/video/Solution_Accelerator_Overview.mp4)


[Azure Resource Setup Video](/video/Azure_Setup_Walkthrough.mp4)


[Click here to access the Hands-on-Lab (AMD64 VM)](/Hands-on-Lab/Hands-on-Lab.md)



# Module Deep Dives

[Part 1 - CIS (capture|inference|store) module](/video/cis_camera_module_part_1.mp4)

[Part 2 - CIS (capture|inference|store) module](/video/cis_camera_module_part_2.mp4)

[Module Twin Configuration Tool module](/video/module_twin_configuration.mp4)

[Model Repo module](/video/model_repo.mp4)

[Custom Dashboard module](/video/custom_dashboard.mp4)

[Image Upload module](/video/image_upload.mp4)

[File Cleanup module](/video/file_cleanup.mp4)

[File Upload/Download Utiliy modules](/video/file_upload_download.mp4)

As you get started on your journey with the ARM64v8 version of the Accelerator, here are a few application notes to keep in mind.  

    1) MS SQL is not available for ARM64, so we're utilizing MySQL for the Edge database. To use this as a persistent datastore, you will want to create the contatainer, and then 'pin' the image created in the Deployment Manifest. Otherwise, the code will create a net new instantiation of MysQL during the build process.

    2) You will need to either 'vi' or 'nano' into the /etc/docker/daemon.json - if this file doesn't exist, go ahead and create it. In here, I recommend adding some implicit values that apply to all containers. Here is the example I use:
    {
        "dns": ["8.8.8.8"],
        "default-runtime": "nvidia",
        "runtimes": {
            "nvidia": {
                "path": "nvidia-container-runtime",
                "runtimeArgs": []
            }
        },
        "log-driver": "json-file",
        "log-opts": {
            "max-size": "10m",
            "max-file": "3"
        }
    }

    3) Once you've built and deployed the Accelerator containers, a new directory at /home/edge_assets will be created on the Edge device. If you want to use the sample models and images contained in this repository for testing, you will need to elevate the permissions of this directory by running:

    sudo chmod -R 777 /home/edge_assets


# About our 'vision' 

This project was built out of necessity - the need to provide for that 'last mile' connectivity of remote vision workloads while also solving for the unique challenges of these use cases. Taking a 'code-first' approach across a number of deep engagements with multiple manufacturing customers, we began to see universal patterns emerge:

1. Build for flexibility: 
 
 Taking a 'code-first' approach meant being deliberate about choices in terms of programming language and the packages used. For this project, we coded everything in Python, and used only open source elements, such as OpenCV for image capture/manipulation and the Open Neural Network Exchange (ONNX) format for vision models. The goal is to provide a usable code base as is, but malleable enough that customers and/or our partners can easily customize it as well.

2. Address the 80%:  

 Within our vision engagements, we found that 80%+ of the use cases revolved around anomaly detection, so the focus of this repository is on object detection. This could be detecting the presence of foreign objects in a production line, detection of errors in assembly, safety infractions such as the lack of PPE or a spill/leakage of product. While image classification and segmentation are also important areas which this repository will expand into eventually, the initial focus is on the majority use case. We will also, in time, provide patterns for integrating Computer Vision services such as OCR to augment capabilities.

3. Plan for model retraining:

 What many of the out-of-the-box solutions fail to plan for is the eventual need for model retraining. A vision solution has to have the ability to augment its datasets, as model drift or 'data' drift happens - it's not a question of if, but a question of when. On the "model" side, perhaps there is a bias in your classes that gets revealed during production inferencing which requires a rebalancing of the class representation. On the 'data' side, it could be that a camera gets knocked out of aligment, the lighting in a plant changes or seasonal changes shift the available natural light, etc.

4. Always think about the Enterprise perspective: 

 The other challenge we've seen consistently in pre-built solutions is the propensity to create yet another silo. Although the lower layers of the ISA-95 network have traditionally been partially or totally 'airgapped' in Manufacturing, the current trend towards Industry 4.0 has created a paradigm shift to connect OT networks to IT systems. Vision systems have also traditionally been in that 'air-gapped' mode, but few have made the transition to Enterprise transparency. From what we've observed, vision analytics systems provide intrinsic value to the front-line plant engineers and workers, but that unique visibility into operations across an Enterprise has value all the way to the C-suite. From the QA to ERP to MES, vision analytics change the way organizations do business for the better.

## License
Copyright (c) Microsoft Corporation

All rights reserved.

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the ""Software""), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED AS IS, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
