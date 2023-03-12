graph LR;
    subgraph Public Subnet
        IGW(Internet Gateway) --> |Route Table| EKSPublicEndpoint[Public IP of EKS Endpoint]
    end
    subgraph Private Subnet
        NATGW[NAT Gateway] --> |Route Table| EKSCluster[Private IP of EKS Endpoint]
    end
    EKSPublicEndpoint --> |Security Group| Jenkins;
    Jenkins --> |Security Group| EKSPublicEndpoint;
    Jenkins --> |Security Group| EKSWorkerNodes;
    EKSWorkerNodes --> |Security Group| Jenkins;
    EKSPublicEndpoint --> |Security Group| EKSControlPlane;
    EKSControlPlane --> |Security Group| EKSPublicEndpoint;
    EKSWorkerNodes --> |Security Group| EKSControlPlane;
    EKSControlPlane --> |Security Group| EKSWorkerNodes;
    subgraph "Availability Zone 1"
        EKSControlPlane --> |EKS Master Nodes| EC2Instance1[EC2 Instance]
        EKSWorkerNodes --> |EKS Worker Nodes| EC2Instance2[EC2 Instance]
    end
    subgraph "Availability Zone 2"
        EKSControlPlane --> |EKS Master Nodes| EC2Instance3[EC2 Instance]
        EKSWorkerNodes --> |EKS Worker Nodes| EC2Instance4[EC2 Instance]
    end

